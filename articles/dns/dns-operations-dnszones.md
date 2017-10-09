---
title: "aaaManage DNS zóny v Azure DNS - prostředí PowerShell | Microsoft Docs"
description: "Můžete spravovat zóny DNS pomocí Azure Powershell. Tento článek popisuje, jak tooupdate, odstraňte a vytvořte zóny DNS na Azure DNS"
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
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 261b89f72213aa9784034d47ff9d1c55a4e80d65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-using-powershell"></a>Jak toomanage zóny DNS pomocí prostředí PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Tento článek ukazuje, jak toomanage DNS zóny pomocí prostředí Azure PowerShell. Můžete také spravovat zóny DNS pomocí hello napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo hello portálu Azure.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóny DNS je vytvořená pomocí hello `New-AzureRmDnsZone` rutiny.

Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

Hello následující příklad ukazuje, jak toocreate DNS zóny se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a>Získat zóny DNS

tooretrieve zóny DNS, použijte hello `Get-AzureRmDnsZone` rutiny. Tato operace vrátí objekt odpovídající tooan existující zónu v Azure DNS zóny DNS. Hello objektu obsahuje data o hello zóny (například hello počet sad záznamů), ale neobsahuje sad záznamů hello sami (viz `Get-AzureRmDnsRecordSet`).

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

Díky vynechání název zóny hello z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón ve skupině prostředků. Tato operace vrátí pole objektů zóny.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Díky vynechání hello název zóny a název skupiny prostředků hello z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón v hello předplatného Azure.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Aktualizaci zóny DNS

Změní tooa prostředků zónu DNS můžete provést pomocí `Set-AzureRmDnsZone`. Tato rutina nelze aktualizovat všechny hello sad záznamů DNS v rámci zóny hello (najdete v části [jak záznamy DNS tooManage](dns-operations-recordsets.md)). Použije se jenom tooupdate vlastnosti prostředku zóny hello, sám sebe. Vlastnosti Hello zapisovatelné zóny jsou aktuálně omezená toohello [Azure Resource Manager, značky' pro prostředek zóny hello](dns-zones-records.md#tags).

Použijte jednu z následujících dvou způsobů tooupdate hello zónu DNS:

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group"></a>Zadejte hello zóny pomocí hello zóny název nebo skupině prostředků

Tento přístup nahradí hello hodnoty zadané existující zóna značky hello.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-hello-zone-using-a-zone-object"></a>Zadejte hello zóny pomocí objekt $zone

Tento přístup načte hello existující objekt zóny, upraví hello značky a pak provádí změny hello. Tímto způsobem je možné zachovat stávající značky.

```powershell
# Get hello zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

Při použití `Set-AzureRmDnsZone` s objektem $zone [kontroluje Etag](dns-zones-records.md#etags) používají tooensure souběžných změny se nepřepíšou. Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Zóny DNS lze odstranit pomocí hello `Remove-AzureRmDnsZone` rutiny.

> [!NOTE]
> Odstranění zóny DNS také odstraní všechny záznamy DNS v rámci zóny hello. Tuto operaci nelze vrátit zpět. Pokud zónu DNS hello se používá, službám pomocí hello zóny se nezdaří, při odstranění zóny hello.
>
>najdete v části tooprotect proti náhodnému zóny odstranění [jak tooprotect DNS zóny a zaznamenává](dns-protect-zones-recordsets.md).


Použijte jednu z následujících dvou způsobů toodelete hello zónu DNS:

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group-name"></a>Zadejte hello zóny pomocí hello název zóny a název skupiny prostředků

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-hello-zone-using-a-zone-object"></a>Zadejte hello zóny pomocí objekt $zone

Můžete zadat toobe zóny hello odstranit pomocí `$zone` objekt vrácený `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Hello zóny objekt lze také přesměrovat místo předávány jako parametr:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

Stejně jako u `Set-AzureRmDnsZone`, zadání hello pomocí zóny `$zone` objektu umožňuje Značka Etag kontroluje změny souběžných tooensure nebudou odstraněny. Použití hello `-Overwrite` přepínač toosuppress těchto kontrol.

## <a name="confirmation-prompts"></a>Potvrzení výzvy

Hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, a `Remove-AzureRmDnsZone` rutiny všechny podporují potvrzení výzvy.

Obě `New-AzureRmDnsZone` a `Set-AzureRmDnsZone` výzvu k potvrzení v případě hello `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší. Z důvodu potenciálně vysoký dopad toohello odstranit zónu DNS hello `Remove-AzureRmDnsZone` rutiny vyzve k potvrzení, pokud hello `$ConfirmPreference` proměnná prostředí PowerShell nemá žádnou hodnotu než `None`.

Od hello výchozí hodnota pro `$ConfirmPreference` je `High`, pouze `Remove-AzureRmDnsZone` vyzve k potvrzení ve výchozím nastavení.

Můžete přepsat hello aktuální `$ConfirmPreference` nastavení pomocí hello `-Confirm` parametr. Pokud zadáte `-Confirm` nebo `-Confirm:$True` , hello rutina vás vyzve k potvrzení před jeho spuštění. Pokud zadáte `-Confirm:$False` , rutina hello nezobrazí výzvu k potvrzení.

Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[spravovat sady záznamů a záznamy](dns-operations-recordsets.md) ve vaší zóně DNS.
<br>
Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).
<br>
Zkontrolujte hello [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).

