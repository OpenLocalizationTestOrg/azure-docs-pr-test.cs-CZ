---
title: "aaaUsing prostředí PowerShell toomanage Traffic Manageru v Azure | Microsoft Docs"
description: "Pomocí prostředí PowerShell pro správce provozu pomocí Azure Resource Manageru"
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 018c37db63beb82fdad54cfd7e13ab3cb645723a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-powershell-toomanage-traffic-manager"></a>Pomocí prostředí PowerShell toomanage Traffic Manager

Azure Resource Manager je hello upřednostňované správu rozhraní pro služby v Azure. Profilů Azure Traffic Manager můžete spravovat pomocí rozhraní API založené na Azure Resource Manager a nástroje.

## <a name="resource-model"></a>Model prostředků

Azure Traffic Manager je konfigurovat pomocí kolekce nastavení nazývá profil Traffic Manageru. Tento profil obsahuje nastavení DNS, nastavení směrování provozu, nastavení monitorování koncového bodu a prochází seznam přenosů toowhich koncové body služby.

Každý profil služby Traffic Manager je reprezentována prostředek typu 'TrafficManagerProfiles'. V hello úroveň rozhraní API REST hello identifikátor URI pro každý profil je následující:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Nastavení prostředí Azure PowerShell

Tyto pokyny použijte Microsoft Azure PowerShell. Hello následující článek vysvětluje, jak tooinstall a konfigurace prostředí Azure PowerShell.

* [Jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview)

Hello příklady v tomto článku předpokládá, že máte existující skupinu prostředků. Můžete vytvořit skupinu prostředků pomocí hello následující příkaz:

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Resource Manager vyžaduje, zda mají všechny skupiny prostředků do umístění. Toto umístění se používá jako výchozí hello pro prostředky vytvořené v příslušné skupině prostředků. Ale vzhledem k tomu, že globální, a ne místní prostředky profil správce provozu se hello Volba umístění skupiny prostředků nemá žádný vliv na Azure Traffic Manager.

## <a name="create-a-traffic-manager-profile"></a>Vytvoření profilu Správce provozu

toocreate profil Traffic Manageru použijte hello `New-AzureRmTrafficManagerProfile` rutiny:

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Hello následující tabulka popisuje parametry hello:

| Parametr | Popis |
| --- | --- |
| Name (Název) |název prostředku Hello hello prostředků profil Traffic Manageru. Profily v nástroji hello stejné skupiny prostředků musí mít jedinečné názvy. Tento název je jiný než název DNS hello používá pro dotazy DNS. |
| Název skupiny prostředků |Název Hello hello prostředků skupiny obsahující hello profil prostředku. |
| TrafficRoutingMethod |Určuje hello směrování provozu metodu použitou toodetermine kterému koncovému bodu je vrácený v odpovědi dotaz DNS. Možné hodnoty jsou 'Výkonu', "Vážená" nebo "Priority". |
| RelativeDnsName |Určuje část názvu hostitele hello název DNS hello poskytované tento profil služby Traffic Manager. Tato hodnota je kombinovat s názvem domény DNS hello používá Azure Traffic Manager tooform hello plně kvalifikovaný název domény (FQDN) hello profilu. Například nastavení hodnoty hello "contoso" bude "contoso.trafficmanager.net." |
| HODNOTA TTL |Určuje hello DNS Time-to-Live (TTL), v sekundách. Tato hodnota TTL informuje překladače hello místní DNS a klienty DNS, jak dlouho toocache odpovědí DNS pro tento profil služby Traffic Manager. |
| MonitorProtocol |Určuje hello protokol toouse toomonitor koncový bod stavu. Možné hodnoty jsou "HTTP" a "HTTPS". |
| MonitorPort |Určuje, že hello TCP port používá toomonitor koncový bod stavu. |
| MonitorPath |Určuje, že název domény pro koncový bod hello cesta relativní toohello používá tooprobe pro koncový bod stavu. |

Hello rutina vytvoří profil Traffic Manageru v Azure a vrátí odpovídající tooPowerShell objekt profilu. V tomto okamžiku hello profil neobsahuje žádné koncové body. Další informace o přidávání profil služby Traffic Manager tooa koncové body najdete v tématu [přidání koncové body Traffic Manager](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Získat profil správce provozu

tooretrieve objekt existující profil Traffic Manageru použijte hello `Get-AzureRmTrafficManagerProfle` rutiny:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Tato rutina vrátí objekt profilu Traffic Manageru.

## <a name="update-a-traffic-manager-profile"></a>Aktualizovat profil správce provozu

Úprava profily Traffic Manager zahrnuje proces krok 3:

1. Načtení hello profil pomocí `Get-AzureRmTrafficManagerProfile` nebo použijte profil hello vrácený `New-AzureRmTrafficManagerProfile`.
2. Upravte profil hello. Můžete přidat a odebrat koncových bodů nebo změňte parametry koncového bodu nebo profilu. Tyto změny jsou offline operace. Měníte jenom místní objekt hello v paměti, která představuje hello profilu.
3. Potvrdit změny pomocí hello `Set-AzureRmTrafficManagerProfile` rutiny.

S výjimkou hello profil RelativeDnsName lze změnit všechny vlastnosti profilu. toochange hello RelativeDnsName, je nutné odstranit profil a nový profil s novým názvem.

Hello následující příklad ukazuje, jak toochange hello profilu TTL:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Existují tři typy koncových bodů Traffic Manager:

1. **Koncové body Azure** jsou služby hostované v Azure
2. **Externí koncové body** jsou služby hostované mimo Azure
3. **Vnořené koncové body** jsou použité tooconstruct vnořené hierarchie profily Traffic Manageru. Vnořené koncové body povolit rozšířené konfigurace směrování provozu pro komplexní aplikace.

Ve všech případech tři koncové body můžete přidat dvěma způsoby:

1. Pomocí kroku 3 postup popsaný výše. Hello Výhodou této metody je, že lze provést několik změn koncového bodu v jednu aktualizaci.
2. Pomocí rutiny New-AzureRmTrafficManagerEndpoint hello. Tato rutina přidá koncový bod tooan existující profil služby Traffic Manager v rámci jedné operace.

## <a name="adding-azure-endpoints"></a>Přidání koncové body Azure

Koncové body Azure odkazovat služby hostované v Azure. Podporuje dva typy koncové body Azure:

1. Azure Web Apps
2. Azure prostředky PublicIpAddress (které můžou být připojené tooa služby Vyrovnávání zatížení nebo virtuální počítač síťový adaptér). Hello PublicIpAddress musí mít název DNS přiřazené toobe použít v Traffic Manageru.

V každém případě:

* Hello služby je zadán pomocí parametru 'targetResourceId' hello `Add-AzureRmTrafficManagerEndpointConfig` nebo `New-AzureRmTrafficManagerEndpoint`.
* Hello "Target" a 'EndpointLocation' jsou implikované hello TargetResourceId.
* Určení hello 'Weight' je volitelné. Váhu používají jenom Pokud je profil hello nakonfigurované toouse hello 'Vážená' směrování provozu metoda. Jinak jsou ignorovány. -Li zadána, hello hodnota musí být číslo v rozsahu od 1 do 1000. Hello výchozí hodnota je '1'.
* Zadání hello "Priority" je volitelný. Priority používají jenom Pokud je profil hello nakonfigurované toouse hello "Priority" směrování provozu metoda. Jinak jsou ignorovány. Platné hodnoty jsou 1 too1000 s nižšími hodnotami, která znamená vyšší prioritu. Pokud je zadán pro jeden koncový bod, musí být zadán pro všechny koncové body. Pokud tento parametr vynechán, se použijí výchozí hodnoty od '1' v hello pořadí, jsou uvedeny hello koncové body.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Příklad 1: Přidání koncových bodů webové aplikace pomocí`Add-AzureRmTrafficManagerEndpointConfig`

V tomto příkladu jsme vytvořte profil Traffic Manageru a přidejte dva koncové body webové aplikace pomocí hello `Add-AzureRmTrafficManagerEndpointConfig` rutiny.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Příklad 2: Přidání koncového bodu publicIpAddress pomocí`New-AzureRmTrafficManagerEndpoint`

V tomto příkladu je přidat prostředek veřejné IP adresy toohello profil služby Traffic Manager. Hello veřejnou IP adresu musí mít název DNS nakonfigurovaný a může být vázána buď toohello síťová karta tohoto virtuálního počítače nebo tooa Vyrovnávání zatížení.

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Přidání externí koncové body

Provoz Manager používá externí koncové body toodirect provoz tooservices hostované mimo Azure. Jak se koncové body Azure externí koncové body přidáním buď pomocí `Add-AzureRmTrafficManagerEndpointConfig` následuje `Set-AzureRmTrafficManagerProfile`, nebo `New-AzureRMTrafficManagerEndpoint`.

Při zadání externí koncové body:

* název domény Hello koncového bodu musí být určen pomocí parametru 'Target' hello
* Pokud se používá metoda směrování provozu "Výkonu" hello, je třeba hello 'EndpointLocation'. V opačném případě je volitelný. Hello hodnota musí být [platný oblast Azure název](https://azure.microsoft.com/regions/).
* Hello 'Vážené' a 'prioritu, jsou volitelné.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Příklad 1: Přidání externí koncové body pomocí `Add-AzureRmTrafficManagerEndpointConfig` a`Set-AzureRmTrafficManagerProfile`

V tomto příkladu jsme vytvořit profil Traffic Manageru, přidejte dva externí koncové body a provést změny hello.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Příklad 2: Přidání externí koncové body pomocí`New-AzureRmTrafficManagerEndpoint`

V tomto příkladu přidáme stávající profil tooan externí koncový bod. profil Hello je zadán pomocí názvy hello profil a prostředků skupiny.

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>Přidání koncových bodů 'vnořené.

Každý profil služby Traffic Manager určuje jednu metodu směrování provozu. Existují však scénáře, které vyžadují složitější směrování provozu, než hello směrování poskytuje jeden profil Traffic Manageru. Lze vnořit profily Traffic Manager toocombine hello výhody víc než jednu metodu směrování provozu. Vnořené profily umožňují toooverride hello výchozí Traffic Manager chování toosupport větší a složitější nasazení aplikací. Podrobné příklady, najdete v části [profily Traffic Manageru vnořené](traffic-manager-nested-profiles.md).

Vnořené koncových bodů jsou nakonfigurované v nadřazené profil hello, pomocí konkrétní koncový bod typu, 'NestedEndpoints'. Při zadávání vnořené koncové body:

* koncový bod Hello je třeba zadat pomocí parametru 'targetResourceId' hello
* Pokud se používá metoda směrování provozu "Výkonu" hello, je třeba hello 'EndpointLocation'. V opačném případě je volitelný. Hello hodnota musí být [platný oblast Azure název](http://azure.microsoft.com/regions/).
* Dobrý den, vážené' a 'prioritu, jsou volitelné, jako koncové body Azure.
* Parametr 'MinChildEndpoints' Hello je volitelné. Hello výchozí hodnota je '1'. Pokud hello počet dostupných koncových bodů klesne pod touto prahovou hodnotou, profil nadřazené hello zvažuje hello podřízené profilu 'snížený výkon a přesměruje přenosy toohello dalších koncových bodů v profilu nadřazené hello.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Příklad 1: Přidání vnořené koncové body pomocí `Add-AzureRmTrafficManagerEndpointConfig` a`Set-AzureRmTrafficManagerProfile`

V tomto příkladu jsme vytvoření nové Traffic Manager podřízenými a nadřazenými profilů, přidat podřízené hello jako nadřazená toohello vnořené koncový bod a provést změny hello.

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Jako stručný výtah v tomto příkladu jsme nepřidali žádné další koncové body toohello nadřazená nebo podřízená profily.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Příklad 2: Přidání vnořené koncové body pomocí`New-AzureRmTrafficManagerEndpoint`

V tomto příkladu přidáme stávající profil podřízené jako existující nadřazené profil tooan vnořené koncový bod. profil Hello je zadán pomocí názvy hello profil a prostředků skupiny.

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="update-a-traffic-manager-endpoint"></a>Aktualizovat koncový bod Traffic Manager

Existují dva způsoby tooupdate existující koncový bod Traffic Manager:

1. Získat pomocí profilu Traffic Manageru hello `Get-AzureRmTrafficManagerProfile`, aktualizovat vlastnosti hello koncového bodu v rámci profilu hello a provést změny hello pomocí `Set-AzureRmTrafficManagerProfile`. Tato metoda má výhod hello je možné tooupdate více než jeden koncový bod v rámci jedné operace.
2. Získat hello Traffic Manager koncový bod pomocí `Get-AzureRmTrafficManagerEndpoint`, aktualizovat vlastnosti hello koncový bod a provést změny hello pomocí `Set-AzureRmTrafficManagerEndpoint`. Tato metoda je jednodušší, protože nevyžaduje indexování do pole hello koncové body v profilu hello.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Příklad 1: Aktualizace koncové body pomocí `Get-AzureRmTrafficManagerProfile` a`Set-AzureRmTrafficManagerProfile`

V tomto příkladu jsme změnit prioritu hello na dva koncové body v rámci stávající profil.

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Příklad 2: Aktualizace k koncový bod pomocí `Get-AzureRmTrafficManagerEndpoint` a`Set-AzureRmTrafficManagerEndpoint`

V tomto příkladu jsme upravit hello váhu jeden koncový bod v existující profil.

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Povolování a zakazování koncových bodů a profily

Traffic Manager umožňuje toobe jednotlivé koncové body povolené a zakázané, a také umožňuje zapnutí a vypnutí celý profilů.
Tyto změny provést hello získávání/aktualizace nebo nastavení koncového bodu nebo profil prostředky. toostreamline tyto běžné operace jsou podporovány také prostřednictvím vyhrazené rutiny.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Příklad 1: Povolení a zakázání profilu Traffic Manageru

použít tooenable profil Traffic Manageru `Enable-AzureRmTrafficManagerProfile`. profil Hello lze zadat pomocí objektu profilu. Hello objektu profilu se dá předat prostřednictvím kanálu hello nebo pomocí hello '-TrafficManagerProfile' parametr. V tomto příkladu určíme hello profil podle názvu skupiny hello profilu a prostředků.

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

toodisable profil Traffic Manageru:

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

rutina Disable-AzureRmTrafficManagerProfile Hello vyzve k potvrzení. Tuto výzvu lze potlačit pomocí hello '-Force' parametr.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Příklad 2: Povolení a zakázání koncový bod Traffic Manager

použít tooenable koncový bod Traffic Manager `Enable-AzureRmTrafficManagerEndpoint`. Existují dva způsoby toospecify hello koncový bod

1. Pomocí objekt TrafficManagerEndpoint předány prostřednictvím kanálu hello nebo pomocí hello '-TrafficManagerEndpoint' parametr
2. Pomocí hello název koncového bodu, typ koncového bodu, název profilu a název skupiny prostředků:

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Podobně toodisable koncový bod Traffic Manager:

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Stejně jako u `Disable-AzureRmTrafficManagerProfile`, hello `Disable-AzureRmTrafficManagerEndpoint` rutiny vyzve k potvrzení. Tuto výzvu lze potlačit pomocí hello '-Force' parametr.

## <a name="delete-a-traffic-manager-endpoint"></a>Odstranit koncový bod Traffic Manager

tooremove jednotlivých koncových bodů, použijte hello `Remove-AzureRmTrafficManagerEndpoint` rutiny:

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Tato rutina vyzve k potvrzení. Tuto výzvu lze potlačit pomocí hello '-Force' parametr.

## <a name="delete-a-traffic-manager-profile"></a>Odstranit profil správce provozu

toodelete profil Traffic Manageru použijte hello `Remove-AzureRmTrafficManagerProfile` rutiny zadání názvy hello profil a prostředek skupiny:

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Tato rutina vyzve k potvrzení. Tuto výzvu lze potlačit pomocí hello '-Force' parametr.

Hello profil toobe odstranit lze zadat také pomocí objektu profilu:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Toto pořadí lze také přesměrovat:

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Další kroky

[Monitorování správce provozu](traffic-manager-monitoring.md)

[Důležité informace o výkonu nástroje Traffic Manager](traffic-manager-performance-considerations.md)
