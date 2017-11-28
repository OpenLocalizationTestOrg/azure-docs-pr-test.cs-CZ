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
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 92f1da660d875c76d5d826669d6c1d12018c3d0a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a><span data-ttu-id="09c0b-104">Správa zón DNS pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="09c0b-104">How to manage DNS Zones using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="09c0b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="09c0b-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="09c0b-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="09c0b-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="09c0b-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="09c0b-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="09c0b-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="09c0b-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="09c0b-109">Tento článek ukazuje, jak spravovat zóny DNS pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="09c0b-109">This article shows you how to manage your DNS zones by using Azure PowerShell.</span></span> <span data-ttu-id="09c0b-110">Můžete také spravovat zóny DNS pomocí napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="09c0b-110">You can also manage your DNS zones using the cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or the Azure portal.</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a><span data-ttu-id="09c0b-111">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="09c0b-111">Create a DNS zone</span></span>

<span data-ttu-id="09c0b-112">Zóna DNS se vytvoří pomocí rutiny `New-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="09c0b-112">A DNS zone is created by using the `New-AzureRmDnsZone` cmdlet.</span></span>

<span data-ttu-id="09c0b-113">Následující příklad vytvoří zónu DNS s názvem *contoso.com* ve skupině prostředků s názvem *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="09c0b-113">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="09c0b-114">Následující příklad ukazuje, jak vytvořit zónu DNS se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*:</span><span class="sxs-lookup"><span data-stu-id="09c0b-114">The following example shows how to create a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="09c0b-115">Získat zóny DNS</span><span class="sxs-lookup"><span data-stu-id="09c0b-115">Get a DNS zone</span></span>

<span data-ttu-id="09c0b-116">Chcete-li načíst zónu DNS, použijte `Get-AzureRmDnsZone` rutiny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-116">To retrieve a DNS zone, use the `Get-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="09c0b-117">Tato operace vrátí objekt zóny DNS odpovídající stávající zónu v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="09c0b-117">This operation returns a DNS zone object corresponding to an existing zone in Azure DNS.</span></span> <span data-ttu-id="09c0b-118">Objekt obsahuje data o zóny (například počet sad záznamů), ale neobsahuje sady záznamů, sami (viz `Get-AzureRmDnsRecordSet`).</span><span class="sxs-lookup"><span data-stu-id="09c0b-118">The object contains data about the zone (such as the number of record sets), but does not contain the record sets themselves (see `Get-AzureRmDnsRecordSet`).</span></span>

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

## <a name="list-dns-zones"></a><span data-ttu-id="09c0b-119">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="09c0b-119">List DNS zones</span></span>

<span data-ttu-id="09c0b-120">Díky vynechání název zóny z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="09c0b-120">By omitting the zone name from `Get-AzureRmDnsZone`, you can enumerate all zones in a resource group.</span></span> <span data-ttu-id="09c0b-121">Tato operace vrátí pole objektů zóny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-121">This operation returns an array of zone objects.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="09c0b-122">Díky vynechání název zóny a názvu skupiny prostředků z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="09c0b-122">By omitting both the zone name and the resource group name from `Get-AzureRmDnsZone`, you can enumerate all zones in the Azure subscription.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="09c0b-123">Aktualizaci zóny DNS</span><span class="sxs-lookup"><span data-stu-id="09c0b-123">Update a DNS zone</span></span>

<span data-ttu-id="09c0b-124">Prostředek zónu DNS můžete měnit pomocí `Set-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="09c0b-124">Changes to a DNS zone resource can be made by using `Set-AzureRmDnsZone`.</span></span> <span data-ttu-id="09c0b-125">Tato rutina nelze aktualizovat všechny sady záznamů DNS v rámci zóny (viz [postup záznamy DNS spravovat](dns-operations-recordsets.md)).</span><span class="sxs-lookup"><span data-stu-id="09c0b-125">This cmdlet does not update any of the DNS record sets within the zone (see [How to Manage DNS records](dns-operations-recordsets.md)).</span></span> <span data-ttu-id="09c0b-126">Toto pravidlo se používá pouze k aktualizaci vlastností prostředku zóny sám sebe.</span><span class="sxs-lookup"><span data-stu-id="09c0b-126">It's only used to update properties of the zone resource itself.</span></span> <span data-ttu-id="09c0b-127">Vlastnosti zapisovatelné zóny jsou aktuálně omezená na [Azure Resource Manager, značky' pro daný prostředek zóny](dns-zones-records.md#tags).</span><span class="sxs-lookup"><span data-stu-id="09c0b-127">The writable zone properties are currently limited to the [Azure Resource Manager ‘tags’ for the zone resource](dns-zones-records.md#tags).</span></span>

<span data-ttu-id="09c0b-128">Použijte jednu z následujících dvou způsobů k aktualizaci zóny DNS:</span><span class="sxs-lookup"><span data-stu-id="09c0b-128">Use one of the following two ways to update a DNS zone:</span></span>

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a><span data-ttu-id="09c0b-129">Zadejte zóny pomocí skupině název a prostředku zóny</span><span class="sxs-lookup"><span data-stu-id="09c0b-129">Specify the zone using the zone name and resource group</span></span>

<span data-ttu-id="09c0b-130">Tento přístup nahradí hodnoty zadané existující zóna značky.</span><span class="sxs-lookup"><span data-stu-id="09c0b-130">This approach replaces the existing zone tags with the values specified.</span></span>

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a><span data-ttu-id="09c0b-131">Zadejte zóny pomocí objekt $zone</span><span class="sxs-lookup"><span data-stu-id="09c0b-131">Specify the zone using a $zone object</span></span>

<span data-ttu-id="09c0b-132">Tento postup načte existující objekt zóny, upraví značek a pak potvrdí změny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-132">This approach retrieves the existing zone object, modifies the tags, and then commits the changes.</span></span> <span data-ttu-id="09c0b-133">Tímto způsobem je možné zachovat stávající značky.</span><span class="sxs-lookup"><span data-stu-id="09c0b-133">In this way, existing tags can be preserved.</span></span>

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

<span data-ttu-id="09c0b-134">Při použití `Set-AzureRmDnsZone` s objektem $zone [kontroluje Etag](dns-zones-records.md#etags) zajišťují souběžných změny se nepřepíšou.</span><span class="sxs-lookup"><span data-stu-id="09c0b-134">When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="09c0b-135">Můžete použít nepovinný `-Overwrite` přepínač tak, aby potlačit těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="09c0b-135">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

## <a name="delete-a-dns-zone"></a><span data-ttu-id="09c0b-136">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="09c0b-136">Delete a DNS Zone</span></span>

<span data-ttu-id="09c0b-137">Zóny DNS lze odstranit pomocí `Remove-AzureRmDnsZone` rutiny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-137">DNS zones can be deleted using the `Remove-AzureRmDnsZone` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="09c0b-138">Odstranění zóny DNS se také odstraní všechny záznamy DNS v rámci zóny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-138">Deleting a DNS zone also deletes all DNS records within the zone.</span></span> <span data-ttu-id="09c0b-139">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="09c0b-139">This operation cannot be undone.</span></span> <span data-ttu-id="09c0b-140">Pokud je zóna DNS se používá, službám pomocí zóny selže při odstranění zóny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-140">If the DNS zone is in use, services using the zone will fail when the zone is deleted.</span></span>
>
><span data-ttu-id="09c0b-141">Chcete-li chránit proti náhodnému zóny odstranění, přečtěte si téma [jak chránit zóny DNS a záznamy](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="09c0b-141">To protect against accidental zone deletion, see [How to protect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>


<span data-ttu-id="09c0b-142">Použijte jednu z následujících dvou způsobů, jak odstranit zónu DNS:</span><span class="sxs-lookup"><span data-stu-id="09c0b-142">Use one of the following two ways to delete a DNS zone:</span></span>

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a><span data-ttu-id="09c0b-143">Zadejte zóny pomocí názvu zóny a název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="09c0b-143">Specify the zone using the zone name and resource group name</span></span>

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a><span data-ttu-id="09c0b-144">Zadejte zóny pomocí objekt $zone</span><span class="sxs-lookup"><span data-stu-id="09c0b-144">Specify the zone using a $zone object</span></span>

<span data-ttu-id="09c0b-145">Můžete zadat pásmo, které chcete odstranit pomocí `$zone` objekt vrácený `Get-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="09c0b-145">You can specify the zone to be deleted using a `$zone` object returned by `Get-AzureRmDnsZone`.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="09c0b-146">Místo předávány jako parametr lze také přesměrovat objekt zóny:</span><span class="sxs-lookup"><span data-stu-id="09c0b-146">The zone object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

<span data-ttu-id="09c0b-147">Stejně jako u `Set-AzureRmDnsZone`, zadání pomocí zóny `$zone` objektu umožňuje Značka Etag kontroly, aby se zajistilo, souběžných změny nebudou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="09c0b-147">As with `Set-AzureRmDnsZone`, specifying the zone using a `$zone` object enables Etag checks to ensure concurrent changes are not deleted.</span></span> <span data-ttu-id="09c0b-148">Použití `-Overwrite` přepínač tak, aby potlačit těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="09c0b-148">Use the `-Overwrite` switch to suppress these checks.</span></span>

## <a name="confirmation-prompts"></a><span data-ttu-id="09c0b-149">Potvrzení výzvy</span><span class="sxs-lookup"><span data-stu-id="09c0b-149">Confirmation prompts</span></span>

<span data-ttu-id="09c0b-150">`New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, A `Remove-AzureRmDnsZone` rutiny všechny podporují potvrzení výzvy.</span><span class="sxs-lookup"><span data-stu-id="09c0b-150">The `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, and `Remove-AzureRmDnsZone` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="09c0b-151">Obě `New-AzureRmDnsZone` a `Set-AzureRmDnsZone` výzvu k potvrzení, pokud `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="09c0b-151">Both `New-AzureRmDnsZone` and `Set-AzureRmDnsZone` prompt for confirmation if the `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="09c0b-152">Z důvodu potenciálně vysoký dopad odstranit zónu DNS `Remove-AzureRmDnsZone` rutiny zobrazí výzvu k potvrzení, pokud `$ConfirmPreference` proměnná prostředí PowerShell nemá žádnou hodnotu než `None`.</span><span class="sxs-lookup"><span data-stu-id="09c0b-152">Due to the potentially high impact of deleting a DNS zone, the `Remove-AzureRmDnsZone` cmdlet prompts for confirmation if the `$ConfirmPreference` PowerShell variable has any value other than `None`.</span></span>

<span data-ttu-id="09c0b-153">Protože výchozí hodnota pro `$ConfirmPreference` je `High`, pouze `Remove-AzureRmDnsZone` vyzve k potvrzení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="09c0b-153">Since the default value for `$ConfirmPreference` is `High`, only `Remove-AzureRmDnsZone` prompts for confirmation by default.</span></span>

<span data-ttu-id="09c0b-154">Můžete přepsat aktuální `$ConfirmPreference` nastavení pomocí `-Confirm` parametr.</span><span class="sxs-lookup"><span data-stu-id="09c0b-154">You can override the current `$ConfirmPreference` setting using the `-Confirm` parameter.</span></span> <span data-ttu-id="09c0b-155">Pokud zadáte `-Confirm` nebo `-Confirm:$True` , rutina vás vyzve k potvrzení před jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="09c0b-155">If you specify `-Confirm` or `-Confirm:$True` , the cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="09c0b-156">Pokud zadáte `-Confirm:$False` , rutina nezobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="09c0b-156">If you specify `-Confirm:$False` , the cmdlet does not prompt you for confirmation.</span></span>

<span data-ttu-id="09c0b-157">Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="09c0b-157">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="09c0b-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="09c0b-158">Next steps</span></span>

<span data-ttu-id="09c0b-159">Zjistěte, jak [spravovat sady záznamů a záznamy](dns-operations-recordsets.md) ve vaší zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="09c0b-159">Learn how to [manage record sets and records](dns-operations-recordsets.md) in your DNS zone.</span></span>
<br>
<span data-ttu-id="09c0b-160">Zjistěte, jak [delegování domény do Azure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="09c0b-160">Learn how to [delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>
<br>
<span data-ttu-id="09c0b-161">Zkontrolujte [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="09c0b-161">Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>

