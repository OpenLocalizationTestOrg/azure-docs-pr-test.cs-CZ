---
title: "Správa zón DNS v Azure DNS - prostředí PowerShell | Microsoft Docs"
description: "Můžete spravovat zóny DNS pomocí Azure Powershell. Tento článek popisuje, jak k aktualizaci, odstranění a vytvoření zóny DNS na Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: gwallace
ms.openlocfilehash: 3f28e70bb6ef46f53375d256a520db40fcb71ad0
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>Správa zón DNS pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Tento článek ukazuje, jak spravovat zóny DNS pomocí prostředí Azure PowerShell. Můžete také spravovat zóny DNS pomocí napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo portálu Azure.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóna DNS se vytvoří pomocí rutiny `New-AzureRmDnsZone`.

Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

Následující příklad ukazuje, jak vytvořit zónu DNS se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Azure DNS teď také podporuje privátní zóny DNS (aktuálně funkce preview).  Příklad toho, jak vytvořit zónu DNS privátní, naleznete v části [začít pracovat s privátní zóny Azure DNS pomocí prostředí PowerShell](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>Získat zóny DNS

Chcete-li načíst zónu DNS, použijte `Get-AzureRmDnsZone` rutiny. Tato operace vrátí objekt zóny DNS odpovídající stávající zónu v Azure DNS. Objekt obsahuje data o zóny (například počet sad záznamů), ale neobsahuje sady záznamů, sami (viz `Get-AzureRmDnsRecordSet`).

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a>Seznam zón DNS

Díky vynechání název zóny z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón ve skupině prostředků. Tato operace vrátí pole objektů zóny.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Díky vynechání název zóny a názvu skupiny prostředků z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón v rámci předplatného Azure.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Aktualizaci zóny DNS

Prostředek zónu DNS můžete měnit pomocí `Set-AzureRmDnsZone`. Tato rutina nelze aktualizovat všechny sady záznamů DNS v rámci zóny (viz [postup záznamy DNS spravovat](dns-operations-recordsets.md)). Toto pravidlo se používá pouze k aktualizaci vlastností prostředku zóny sám sebe. Vlastnosti zapisovatelné zóny jsou aktuálně omezená na [Azure Resource Manager, značky' pro daný prostředek zóny](dns-zones-records.md#tags).

Použijte jednu z následujících dvou způsobů k aktualizaci zóny DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Zadejte zóny pomocí skupině název a prostředku zóny

Tento přístup nahradí hodnoty zadané existující zóna značky.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>Zadejte zóny pomocí objekt $zone

Tento postup načte existující objekt zóny, upraví značek a pak potvrdí změny. Tímto způsobem je možné zachovat stávající značky.

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

Při použití `Set-AzureRmDnsZone` s objektem $zone [kontroluje Etag](dns-zones-records.md#etags) zajišťují souběžných změny se nepřepíšou. Můžete použít nepovinný `-Overwrite` přepínač tak, aby potlačit těchto kontrol.

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Zóny DNS lze odstranit pomocí `Remove-AzureRmDnsZone` rutiny.

> [!NOTE]
> Odstranění zóny DNS se také odstraní všechny záznamy DNS v rámci zóny. Tuto operaci nelze vrátit zpět. Pokud je zóna DNS se používá, službám pomocí zóny selže při odstranění zóny.
>
>Chcete-li chránit proti náhodnému zóny odstranění, přečtěte si téma [jak chránit zóny DNS a záznamy](dns-protect-zones-recordsets.md).


Použijte jednu z následujících dvou způsobů, jak odstranit zónu DNS:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Zadejte zóny pomocí názvu zóny a název skupiny prostředků

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Zadejte zóny pomocí objekt $zone

Můžete zadat pásmo, které chcete odstranit pomocí `$zone` objekt vrácený `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Místo předávány jako parametr lze také přesměrovat objekt zóny:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

Stejně jako u `Set-AzureRmDnsZone`, zadání pomocí zóny `$zone` objektu umožňuje Značka Etag kontroly, aby se zajistilo, souběžných změny nebudou odstraněny. Použití `-Overwrite` přepínač tak, aby potlačit těchto kontrol.

## <a name="confirmation-prompts"></a>Potvrzení výzvy

`New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, A `Remove-AzureRmDnsZone` rutiny všechny podporují potvrzení výzvy.

Obě `New-AzureRmDnsZone` a `Set-AzureRmDnsZone` výzvu k potvrzení, pokud `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší. Z důvodu potenciálně vysoký dopad odstranit zónu DNS `Remove-AzureRmDnsZone` rutiny zobrazí výzvu k potvrzení, pokud `$ConfirmPreference` proměnná prostředí PowerShell nemá žádnou hodnotu než `None`.

Protože výchozí hodnota pro `$ConfirmPreference` je `High`, pouze `Remove-AzureRmDnsZone` vyzve k potvrzení ve výchozím nastavení.

Můžete přepsat aktuální `$ConfirmPreference` nastavení pomocí `-Confirm` parametr. Pokud zadáte `-Confirm` nebo `-Confirm:$True` , rutina vás vyzve k potvrzení před jeho spuštění. Pokud zadáte `-Confirm:$False` , rutina nezobrazí výzvu k potvrzení.

Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Další kroky

Zjistěte, jak [spravovat sady záznamů a záznamy](dns-operations-recordsets.md) ve vaší zóně DNS.
<br>
Zjistěte, jak [delegování domény do Azure DNS](dns-domain-delegation.md).
<br>
Zkontrolujte [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).

