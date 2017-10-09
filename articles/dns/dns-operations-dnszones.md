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
# <a name="how-toomanage-dns-zones-using-powershell"></a><span data-ttu-id="674ae-104">Jak toomanage zóny DNS pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="674ae-104">How toomanage DNS Zones using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="674ae-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="674ae-105">Portal</span></span>](dns-operations-dnszones-portal.md)
> * [<span data-ttu-id="674ae-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="674ae-106">PowerShell</span></span>](dns-operations-dnszones.md)
> * [<span data-ttu-id="674ae-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="674ae-107">Azure CLI 1.0</span></span>](dns-operations-dnszones-cli-nodejs.md)
> * [<span data-ttu-id="674ae-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="674ae-108">Azure CLI 2.0</span></span>](dns-operations-dnszones-cli.md)

<span data-ttu-id="674ae-109">Tento článek ukazuje, jak toomanage DNS zóny pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="674ae-109">This article shows you how toomanage your DNS zones by using Azure PowerShell.</span></span> <span data-ttu-id="674ae-110">Můžete také spravovat zóny DNS pomocí hello napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="674ae-110">You can also manage your DNS zones using hello cross-platform [Azure CLI](dns-operations-dnszones-cli.md) or hello Azure portal.</span></span>

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a><span data-ttu-id="674ae-111">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="674ae-111">Create a DNS zone</span></span>

<span data-ttu-id="674ae-112">Zóny DNS je vytvořená pomocí hello `New-AzureRmDnsZone` rutiny.</span><span class="sxs-lookup"><span data-stu-id="674ae-112">A DNS zone is created by using hello `New-AzureRmDnsZone` cmdlet.</span></span>

<span data-ttu-id="674ae-113">Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="674ae-113">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="674ae-114">Hello následující příklad ukazuje, jak toocreate DNS zóny se dvěma [Azure Resource Manager značky](dns-zones-records.md#tags), *project = demo* a *env = test*:</span><span class="sxs-lookup"><span data-stu-id="674ae-114">hello following example shows how toocreate a DNS zone with two [Azure Resource Manager tags](dns-zones-records.md#tags), *project = demo* and *env = test*:</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a><span data-ttu-id="674ae-115">Získat zóny DNS</span><span class="sxs-lookup"><span data-stu-id="674ae-115">Get a DNS zone</span></span>

<span data-ttu-id="674ae-116">tooretrieve zóny DNS, použijte hello `Get-AzureRmDnsZone` rutiny.</span><span class="sxs-lookup"><span data-stu-id="674ae-116">tooretrieve a DNS zone, use hello `Get-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="674ae-117">Tato operace vrátí objekt odpovídající tooan existující zónu v Azure DNS zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="674ae-117">This operation returns a DNS zone object corresponding tooan existing zone in Azure DNS.</span></span> <span data-ttu-id="674ae-118">Hello objektu obsahuje data o hello zóny (například hello počet sad záznamů), ale neobsahuje sad záznamů hello sami (viz `Get-AzureRmDnsRecordSet`).</span><span class="sxs-lookup"><span data-stu-id="674ae-118">hello object contains data about hello zone (such as hello number of record sets), but does not contain hello record sets themselves (see `Get-AzureRmDnsRecordSet`).</span></span>

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

## <a name="list-dns-zones"></a><span data-ttu-id="674ae-119">Seznam zón DNS</span><span class="sxs-lookup"><span data-stu-id="674ae-119">List DNS zones</span></span>

<span data-ttu-id="674ae-120">Díky vynechání název zóny hello z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="674ae-120">By omitting hello zone name from `Get-AzureRmDnsZone`, you can enumerate all zones in a resource group.</span></span> <span data-ttu-id="674ae-121">Tato operace vrátí pole objektů zóny.</span><span class="sxs-lookup"><span data-stu-id="674ae-121">This operation returns an array of zone objects.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

<span data-ttu-id="674ae-122">Díky vynechání hello název zóny a název skupiny prostředků hello z `Get-AzureRmDnsZone`, můžete vytvořit výčet všech zón v hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="674ae-122">By omitting both hello zone name and hello resource group name from `Get-AzureRmDnsZone`, you can enumerate all zones in hello Azure subscription.</span></span>

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a><span data-ttu-id="674ae-123">Aktualizaci zóny DNS</span><span class="sxs-lookup"><span data-stu-id="674ae-123">Update a DNS zone</span></span>

<span data-ttu-id="674ae-124">Změní tooa prostředků zónu DNS můžete provést pomocí `Set-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="674ae-124">Changes tooa DNS zone resource can be made by using `Set-AzureRmDnsZone`.</span></span> <span data-ttu-id="674ae-125">Tato rutina nelze aktualizovat všechny hello sad záznamů DNS v rámci zóny hello (najdete v části [jak záznamy DNS tooManage](dns-operations-recordsets.md)).</span><span class="sxs-lookup"><span data-stu-id="674ae-125">This cmdlet does not update any of hello DNS record sets within hello zone (see [How tooManage DNS records](dns-operations-recordsets.md)).</span></span> <span data-ttu-id="674ae-126">Použije se jenom tooupdate vlastnosti prostředku zóny hello, sám sebe.</span><span class="sxs-lookup"><span data-stu-id="674ae-126">It's only used tooupdate properties of hello zone resource itself.</span></span> <span data-ttu-id="674ae-127">Vlastnosti Hello zapisovatelné zóny jsou aktuálně omezená toohello [Azure Resource Manager, značky' pro prostředek zóny hello](dns-zones-records.md#tags).</span><span class="sxs-lookup"><span data-stu-id="674ae-127">hello writable zone properties are currently limited toohello [Azure Resource Manager ‘tags’ for hello zone resource](dns-zones-records.md#tags).</span></span>

<span data-ttu-id="674ae-128">Použijte jednu z následujících dvou způsobů tooupdate hello zónu DNS:</span><span class="sxs-lookup"><span data-stu-id="674ae-128">Use one of hello following two ways tooupdate a DNS zone:</span></span>

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group"></a><span data-ttu-id="674ae-129">Zadejte hello zóny pomocí hello zóny název nebo skupině prostředků</span><span class="sxs-lookup"><span data-stu-id="674ae-129">Specify hello zone using hello zone name and resource group</span></span>

<span data-ttu-id="674ae-130">Tento přístup nahradí hello hodnoty zadané existující zóna značky hello.</span><span class="sxs-lookup"><span data-stu-id="674ae-130">This approach replaces hello existing zone tags with hello values specified.</span></span>

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-hello-zone-using-a-zone-object"></a><span data-ttu-id="674ae-131">Zadejte hello zóny pomocí objekt $zone</span><span class="sxs-lookup"><span data-stu-id="674ae-131">Specify hello zone using a $zone object</span></span>

<span data-ttu-id="674ae-132">Tento přístup načte hello existující objekt zóny, upraví hello značky a pak provádí změny hello.</span><span class="sxs-lookup"><span data-stu-id="674ae-132">This approach retrieves hello existing zone object, modifies hello tags, and then commits hello changes.</span></span> <span data-ttu-id="674ae-133">Tímto způsobem je možné zachovat stávající značky.</span><span class="sxs-lookup"><span data-stu-id="674ae-133">In this way, existing tags can be preserved.</span></span>

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

<span data-ttu-id="674ae-134">Při použití `Set-AzureRmDnsZone` s objektem $zone [kontroluje Etag](dns-zones-records.md#etags) používají tooensure souběžných změny se nepřepíšou.</span><span class="sxs-lookup"><span data-stu-id="674ae-134">When using `Set-AzureRmDnsZone` with a $zone object, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="674ae-135">Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="674ae-135">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

## <a name="delete-a-dns-zone"></a><span data-ttu-id="674ae-136">Odstranit zónu DNS</span><span class="sxs-lookup"><span data-stu-id="674ae-136">Delete a DNS Zone</span></span>

<span data-ttu-id="674ae-137">Zóny DNS lze odstranit pomocí hello `Remove-AzureRmDnsZone` rutiny.</span><span class="sxs-lookup"><span data-stu-id="674ae-137">DNS zones can be deleted using hello `Remove-AzureRmDnsZone` cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="674ae-138">Odstranění zóny DNS také odstraní všechny záznamy DNS v rámci zóny hello.</span><span class="sxs-lookup"><span data-stu-id="674ae-138">Deleting a DNS zone also deletes all DNS records within hello zone.</span></span> <span data-ttu-id="674ae-139">Tuto operaci nelze vrátit zpět.</span><span class="sxs-lookup"><span data-stu-id="674ae-139">This operation cannot be undone.</span></span> <span data-ttu-id="674ae-140">Pokud zónu DNS hello se používá, službám pomocí hello zóny se nezdaří, při odstranění zóny hello.</span><span class="sxs-lookup"><span data-stu-id="674ae-140">If hello DNS zone is in use, services using hello zone will fail when hello zone is deleted.</span></span>
>
><span data-ttu-id="674ae-141">najdete v části tooprotect proti náhodnému zóny odstranění [jak tooprotect DNS zóny a zaznamenává](dns-protect-zones-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="674ae-141">tooprotect against accidental zone deletion, see [How tooprotect DNS zones and records](dns-protect-zones-recordsets.md).</span></span>


<span data-ttu-id="674ae-142">Použijte jednu z následujících dvou způsobů toodelete hello zónu DNS:</span><span class="sxs-lookup"><span data-stu-id="674ae-142">Use one of hello following two ways toodelete a DNS zone:</span></span>

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group-name"></a><span data-ttu-id="674ae-143">Zadejte hello zóny pomocí hello název zóny a název skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="674ae-143">Specify hello zone using hello zone name and resource group name</span></span>

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-hello-zone-using-a-zone-object"></a><span data-ttu-id="674ae-144">Zadejte hello zóny pomocí objekt $zone</span><span class="sxs-lookup"><span data-stu-id="674ae-144">Specify hello zone using a $zone object</span></span>

<span data-ttu-id="674ae-145">Můžete zadat toobe zóny hello odstranit pomocí `$zone` objekt vrácený `Get-AzureRmDnsZone`.</span><span class="sxs-lookup"><span data-stu-id="674ae-145">You can specify hello zone toobe deleted using a `$zone` object returned by `Get-AzureRmDnsZone`.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

<span data-ttu-id="674ae-146">Hello zóny objekt lze také přesměrovat místo předávány jako parametr:</span><span class="sxs-lookup"><span data-stu-id="674ae-146">hello zone object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

<span data-ttu-id="674ae-147">Stejně jako u `Set-AzureRmDnsZone`, zadání hello pomocí zóny `$zone` objektu umožňuje Značka Etag kontroluje změny souběžných tooensure nebudou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="674ae-147">As with `Set-AzureRmDnsZone`, specifying hello zone using a `$zone` object enables Etag checks tooensure concurrent changes are not deleted.</span></span> <span data-ttu-id="674ae-148">Použití hello `-Overwrite` přepínač toosuppress těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="674ae-148">Use hello `-Overwrite` switch toosuppress these checks.</span></span>

## <a name="confirmation-prompts"></a><span data-ttu-id="674ae-149">Potvrzení výzvy</span><span class="sxs-lookup"><span data-stu-id="674ae-149">Confirmation prompts</span></span>

<span data-ttu-id="674ae-150">Hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, a `Remove-AzureRmDnsZone` rutiny všechny podporují potvrzení výzvy.</span><span class="sxs-lookup"><span data-stu-id="674ae-150">hello `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, and `Remove-AzureRmDnsZone` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="674ae-151">Obě `New-AzureRmDnsZone` a `Set-AzureRmDnsZone` výzvu k potvrzení v případě hello `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="674ae-151">Both `New-AzureRmDnsZone` and `Set-AzureRmDnsZone` prompt for confirmation if hello `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="674ae-152">Z důvodu potenciálně vysoký dopad toohello odstranit zónu DNS hello `Remove-AzureRmDnsZone` rutiny vyzve k potvrzení, pokud hello `$ConfirmPreference` proměnná prostředí PowerShell nemá žádnou hodnotu než `None`.</span><span class="sxs-lookup"><span data-stu-id="674ae-152">Due toohello potentially high impact of deleting a DNS zone, hello `Remove-AzureRmDnsZone` cmdlet prompts for confirmation if hello `$ConfirmPreference` PowerShell variable has any value other than `None`.</span></span>

<span data-ttu-id="674ae-153">Od hello výchozí hodnota pro `$ConfirmPreference` je `High`, pouze `Remove-AzureRmDnsZone` vyzve k potvrzení ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="674ae-153">Since hello default value for `$ConfirmPreference` is `High`, only `Remove-AzureRmDnsZone` prompts for confirmation by default.</span></span>

<span data-ttu-id="674ae-154">Můžete přepsat hello aktuální `$ConfirmPreference` nastavení pomocí hello `-Confirm` parametr.</span><span class="sxs-lookup"><span data-stu-id="674ae-154">You can override hello current `$ConfirmPreference` setting using hello `-Confirm` parameter.</span></span> <span data-ttu-id="674ae-155">Pokud zadáte `-Confirm` nebo `-Confirm:$True` , hello rutina vás vyzve k potvrzení před jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="674ae-155">If you specify `-Confirm` or `-Confirm:$True` , hello cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="674ae-156">Pokud zadáte `-Confirm:$False` , rutina hello nezobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="674ae-156">If you specify `-Confirm:$False` , hello cmdlet does not prompt you for confirmation.</span></span>

<span data-ttu-id="674ae-157">Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="674ae-157">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="674ae-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="674ae-158">Next steps</span></span>

<span data-ttu-id="674ae-159">Zjistěte, jak příliš[spravovat sady záznamů a záznamy](dns-operations-recordsets.md) ve vaší zóně DNS.</span><span class="sxs-lookup"><span data-stu-id="674ae-159">Learn how too[manage record sets and records](dns-operations-recordsets.md) in your DNS zone.</span></span>
<br>
<span data-ttu-id="674ae-160">Zjistěte, jak příliš[delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="674ae-160">Learn how too[delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>
<br>
<span data-ttu-id="674ae-161">Zkontrolujte hello [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="674ae-161">Review hello [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>

