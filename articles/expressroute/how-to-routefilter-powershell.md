---
title: "Nastavit filtry směrování pro partnerský vztah Azure ExpressRoute Microsoftu: prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak filtry tooconfigure trasy pro Microsoft Peering pomocí prostředí PowerShell"
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 395bcf7d6ad43c643ff041b154f8e4b797083e7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-route-filters-for-microsoft-peering"></a>Konfigurace filtrů směrování pro partnerský vztah Microsoftu

Filtry tras jsou způsob tooconsume podmnožinu podporované služby prostřednictvím partnerského vztahu Microsoftu. Hello kroky v této nápovědě článku můžete nakonfigurovat a spravovat filtry tras pro okruhy ExpressRoute.

Dynamics 365 služeb a služeb Office 365 například Exchange Online, SharePoint Online a Skype pro firmy, jsou přístupné prostřednictvím partnerského vztahu Microsoftu hello. Pokud partnerský vztah Microsoftu je nakonfigurován v okruhu ExpressRoute, jsou všechny služby související toothese předpony inzerované prostřednictvím hello relací protokolu BGP, které jsou vytvořeny. Hodnota komunity protokolu BGP je připojený tooevery předponu tooidentify hello služba, která je nabízeným přes hello předponu. Seznam hodnot komunity protokolu BGP hello a hello služeb jsou mapovány najdete v tématu [komunit protokolu BGP](expressroute-routing.md#bgp).

Pokud budete potřebovat připojení tooall služeb, jsou velký počet předpon inzerovaných prostřednictvím protokolu BGP. Tím se výrazně zvyšuje velikost hello hello směrovací tabulky udržuje pomocí směrovačů v rámci vaší sítě. Pokud máte v plánu tooconsume pouze podmnožinu službám nabízeným přes partnerské vztahy Microsoft, můžete snížit velikost hello vaší směrovací tabulky dvěma způsoby. Můžete:

- Filtrovat nežádoucí předpony použitím filtry tras na komunit protokolu BGP. Tento postup standardní sítě a běžně používá v rámci mnoho sítí.

- Definovat filtry tras a použít tooyour okruh ExpressRoute. Filtr trasy je nový prostředek, který vám umožní vybrat hello seznam služeb můžete naplánovat tooconsume prostřednictvím partnerského vztahu Microsoftu. ExpressRoute směrovače odeslat pouze hello seznam předpon, které patří služby toohello stanovených ve filtru hello trasy.

### <a name="about"></a>O filtrech směrování

Pokud partnerský vztah Microsoftu je nakonfigurován na váš okruh ExpressRoute, hello Microsoft hraniční směrovače vytvořte dvojici relací protokolu BGP s hello hraniční směrovače (váš nebo váš poskytovatel připojení). Trasy se inzerovaný tooyour sítě. tooenable trasy oznámení o inzerovaném programu tooyour sítě, je nutné přidružit filtr trasy.

Filtr trasy umožňuje identifikovat služby, které mají tooconsume prostřednictvím partnerského vztahu Microsoftu okruhu ExpressRoute. Je to v podstatě prázdný seznam všech hodnot komunity protokolu BGP hello. Prostředek trasy filtru je definovaný a připojené tooan okruh ExpressRoute, budete všech předpon, které mapují hodnoty komunity protokolu BGP toohello inzerovaný tooyour sítě.

filtry tras toobe možné tooattach se službami Office 365 na ně, musí mít služby Office 365 tooconsume autorizace prostřednictvím ExpressRoute. Pokud si nejste oprávnění tooconsume Office 365 služby prostřednictvím ExpressRoute, filtry tras tooattach hello operace selže. Další informace o procesu autorizačního hello najdete v tématu [Azure ExpressRoute pro Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd). 365 služeb připojení k tooDynamics nevyžaduje žádné předchozí autorizace.

> [!IMPORTANT]
> Partnerského vztahu Microsoftu okruhy ExpressRoute, které byly nakonfigurovány předchozí tooAugust 1, 2017 budou mít všechny služby předpony inzerované prostřednictvím partnerského vztahu Microsoftu, i když nejsou definovány filtry tras. Okruhy ExpressRoute, které jsou nakonfigurované na nebo po 1 srpen 2017 partnerského vztahu Microsoftu nebude mít všechny předpony inzerované, dokud nebude připojené filtr trasy toohello okruh.
> 
> 

### <a name="workflow"></a>Pracovní postup

možnost toosuccessfully toobe připojit tooservices prostřednictvím partnerského vztahu Microsoftu, je třeba provést následující kroky konfigurace hello:

- Musí mít aktivní okruh ExpressRoute, který má Microsoft partnerský vztah zřízené. Následující pokyny tooaccomplish hello můžete použít tyto úlohy:
  - [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.
  - [Vytvoření partnerského vztahu Microsoftu](expressroute-circuit-peerings.md) Pokud spravujete hello přímo v relaci protokolu BGP. Nebo váš poskytovatel připojení zřídit partnerský vztah Microsoftu pro váš okruh.

-  Musíte vytvořit a konfigurovat filtr trasy.
    - Identifikovat hello služby services s tooconsume prostřednictvím partnerského vztahu Microsoftu
    - Identifikovat hello seznam hodnot komunity protokolu BGP souvisejících se službou hello
    - Vytvoření pravidlo tooallow hello předponu seznamu odpovídající hello hodnotami komunity protokolu BGP

-  Je nutné připojit okruh ExpressRoute toohello filtr trasy hello.

## <a name="before-you-begin"></a>Než začnete

Před zahájením konfigurace, ujistěte se, že splňujete hello následující kritéria:

 - Nainstalujte nejnovější verzi rutin Powershellu pro Azure Resource Manager hello hello. Další informace najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShelll](/powershell/azure/install-azurerm-ps).

  > [!NOTE]
  > Stažení nejnovější verze hello z Galerie prostředí PowerShell text hello, nikoli pomocí hello Instalační služby. Hello instalační program aktuálně nepodporuje hello požadované rutiny.
  > 

 - Zkontrolujte hello [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.

 - Musí mít aktivní okruh ExpressRoute. Postupujte podle pokynů hello příliš[vytvoření okruhu ExpressRoute](expressroute-howto-circuit-arm.md) a mít okruh hello povolený vaším poskytovatelem připojení, než budete pokračovat. Hello okruh ExpressRoute musí být ve stavu zřízený a povolený.

 - Musíte mít active partnerský vztah Microsoftu. Postupujte podle pokynů v [vytvoření a úprava konfigurace partnerského vztahu](expressroute-circuit-peerings.md)

### <a name="log-in-tooyour-azure-account"></a>Přihlaste se tooyour účet Azure

Před zahájením této konfigurace, musíte být přihlášení tooyour účet Azure. Hello rutina vás vyzve k zadání hello přihlašovací údaje k účtu Azure. Po přihlášení stahování nastavení svého účtu tak, aby byly k dispozici tooAzure prostředí PowerShell.

Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními a připojte tooyour účtu. Použijte následující příklad toohelp, ke kterým se připojujete hello:

```powershell
Login-AzureRmAccount
```

Pokud máte víc předplatných Azure, zkontrolujte hello předplatná pro účet hello.

```powershell
Get-AzureRmSubscription
```

Zadejte hello předplatné, které chcete toouse.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="prefixes"></a>Krok 1. Získat seznam předpon a hodnotami komunity protokolu BGP

### <a name="1-get-a-list-of-bgp-community-values"></a>1. Získat seznam hodnot komunity protokolu BGP

Použijte následující rutinu tooget hello seznam hodnot komunity protokolu BGP souvisejících se službou přístupné prostřednictvím partnerského vztahu Microsoftu hello a hello seznam předpon, které jsou s nimi spojených:

```powershell
Get-AzureRmBgpServiceCommunity
```
### <a name="2-make-a-list-of-hello-values-that-you-want-toouse"></a>2. Zkontrolujte seznam hello hodnot, které chcete toouse

Zkontrolujte seznam hodnotami komunity protokolu BGP, které chcete toouse ve filtru hello trasy. Jako příklad je hello hodnota komunity protokolu BGP pro služby Dynamics 365 12076:5040.

## <a name="filter"></a>Krok 2. Vytvořte filtr trasy a pravidlo filtru

Filtr trasy může mít pouze jedno pravidlo a hello pravidlo musí být typu 'Povolit'. Toto pravidlo může mít seznam hodnot komunity protokolu BGP s ním spojená.

### <a name="1-create-a-route-filter"></a>1. Vytvoření filtru trasy

Nejprve vytvořte filtr hello trasy. příkaz Hello 'New-AzureRmRouteFilter, pouze vytvoří prostředek trasy filtru. Po vytvoření hello prostředků, musíte pak vytvořit pravidlo a připojte ji toohello trasy filtru objektu. Spusťte následující příkaz toocreate hello prostředek trasy filtru:

```powershell
New-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup" -Location "West US"
```

### <a name="2-create-a-filter-rule"></a>2. Vytvořit pravidlo filtru

Sadu komunit protokolu BGP jako seznam oddělený čárkami, můžete zadat, jak je znázorněno v příkladu hello. Spusťte následující příkaz toocreate hello nové pravidlo:
 
```powershell
$rule = New-AzureRmRouteFilterRuleConfig -Name "Allow-EXO-D365" -Access Allow -RouteFilterRuleType Community -CommunityList "12076:5010,12076:5040"
```

### <a name="3-add-hello-rule-toohello-route-filter"></a>3. Přidat filtr trasy toohello pravidlo hello

Spusťte následující příkaz tooadd hello filtru pravidlo toohello trasy filtru hello:
 
```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.Rules.Add($rule)
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="attach"></a>Krok 3. Připojte okruh ExpressRoute tooan filtr trasy hello

Spusťte následující příkaz tooattach hello trasy filtru toohello okruh ExpressRoute, za předpokladu, že máte jenom partnerského vztahu Microsoft hello:

```powershell
$ckt.Peerings[0].RouteFilter = $routefilter 
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="getproperties"></a>Vlastnosti hello tooget filtru trasy

Vlastnosti hello tooget filtru trasy, hello použijte následující kroky:

1. Spusťte následující příkaz tooget hello trasy filtru prostředků hello:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  ```
2. Získáte pravidla filtru hello trasy pro prostředek trasy filtru hello spuštěním hello následující příkaz:

  ```powershell
  $routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
  $rule = $routefilter.Rules[0]
  ```

## <a name="updateproperties"></a>Vlastnosti hello tooupdate filtru trasy

Pokud hello trasy filtru je již připojen tooa okruhu seznamu komunity protokolu BGP toohello aktualizace automaticky šíří oznámení o inzerovaném programu změny příslušné předpony prostřednictvím relace protokolu BGP hello navázat. Můžete aktualizovat seznam komunity protokolu BGP hello filtru trasu pomocí hello následující příkaz:

```powershell
$routefilter = Get-AzureRmRouteFilter -Name "RouteFilterName" -ResourceGroupName "ExpressRouteResourceGroupName"
$routefilter.rules[0].Communities = "12076:5030", "12076:5040"
Set-AzureRmRouteFilter -RouteFilter $routefilter
```

## <a name="detach"></a>toodetach filtr trasy z okruhu ExpressRoute

Jakmile filtr trasy odpojený od hello okruh ExpressRoute, jsou bez předpony inzerované prostřednictvím relace protokolu BGP hello. Můžete odpojit filtr trasy z okruhu ExpressRoute pomocí hello následující příkaz:
  
```powershell
$ckt.Peerings[0].RouteFilter = $null
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="delete"></a>toodelete filtr trasy

Můžete pouze odstranění filtru trasy, pokud není připojen tooany okruh. Zkontrolujte tento filtr trasy hello není připojen tooany okruhu před pokusem o toodelete ho. Odstraněním trasy filtrovat pomocí hello následující příkaz:

```powershell
Remove-AzureRmRouteFilter -Name "MyRouteFilter" -ResourceGroupName "MyResourceGroup"
```

## <a name="next-steps"></a>Další kroky

Další informace o ExpressRoute najdete v tématu hello [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
