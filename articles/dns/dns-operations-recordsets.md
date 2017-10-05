---
title: "Správa záznamů DNS v Azure DNS pomocí Azure PowerShell | Microsoft Docs"
description: "Správa sad záznamů DNS a záznamy v Azure DNS při hostování vaší domény ve službě Azure DNS. Všechny příkazy prostředí PowerShell pro operace na sady záznamů a záznamy."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: 2962e30e5d9c60b8e786e2ba79647cabfc5925cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a><span data-ttu-id="48742-104">Správa záznamů DNS a sady záznamů v Azure DNS pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="48742-104">Manage DNS records and recordsets in Azure DNS using Azure PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="48742-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="48742-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="48742-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="48742-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="48742-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="48742-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="48742-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="48742-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="48742-109">Tento článek ukazuje, jak spravovat záznamy DNS pro zónu DNS pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48742-109">This article shows you how to manage DNS records for your DNS zone by using Azure PowerShell.</span></span> <span data-ttu-id="48742-110">Záznamy DNS se taky dají spravovat pomocí napříč platformami [rozhraní příkazového řádku Azure](dns-operations-recordsets-cli.md) nebo [portál Azure](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="48742-110">DNS records can also be managed by using the cross-platform [Azure CLI](dns-operations-recordsets-cli.md) or the [Azure portal](dns-operations-recordsets-portal.md).</span></span>

<span data-ttu-id="48742-111">V příkladech v tomto článku předpokládá, že už máte [nainstalovaný Azure PowerShell, přihlášení a vytvořit zónu DNS](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="48742-111">The examples in this article assume you have already [installed Azure PowerShell, signed in, and created a DNS zone](dns-operations-dnszones.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="48742-112">Úvod</span><span class="sxs-lookup"><span data-stu-id="48742-112">Introduction</span></span>

<span data-ttu-id="48742-113">Před vytvářením záznamů DNS v DNS Azure je nejprve nutné pochopit, jak DNS Azure organizuje záznamy DNS v sadách záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-113">Before creating DNS records in Azure DNS, you first need to understand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="48742-114">Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="48742-114">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>


## <a name="create-a-new-dns-record"></a><span data-ttu-id="48742-115">Vytvořit nový záznam DNS</span><span class="sxs-lookup"><span data-stu-id="48742-115">Create a new DNS record</span></span>

<span data-ttu-id="48742-116">Pokud nový záznam se stejným názvem a typem jako stávající záznam, budete muset [ho přidat do existující sady záznamů](#add-a-record-to-an-existing-record-set).</span><span class="sxs-lookup"><span data-stu-id="48742-116">If your new record has the same name and type as an existing record, you need to [add it to the existing record set](#add-a-record-to-an-existing-record-set).</span></span> <span data-ttu-id="48742-117">Pokud nový záznam má jiný název a typ, který má všechny existující záznamy, budete muset vytvořit novou sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-117">If your new record has a different name and type to all existing records, you need to create a new record set.</span></span> 

### <a name="create-a-records-in-a-new-record-set"></a><span data-ttu-id="48742-118">Vytvořit "A" záznamy v nové sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-118">Create 'A' records in a new record set</span></span>

<span data-ttu-id="48742-119">Sady záznamů vytvoříte pomocí rutiny `New-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="48742-119">You create record sets by using the `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="48742-120">Při vytváření sady záznamů, budete muset zadat sady záznamu název zóny, čas k live (TTL), typ záznamu a záznamy, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="48742-120">When creating a record set, you need to specify the record set name, the zone, the time to live (TTL), the record type, and the records to be created.</span></span>

<span data-ttu-id="48742-121">Parametry pro přidání záznamů do sady záznamů se liší podle typu sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-121">The parameters for adding records to a record set vary depending on the type of the record set.</span></span> <span data-ttu-id="48742-122">Například pokud používáte sadu záznamů typu "A", budete muset zadat IP adresu pomocí parametru `-IPv4Address`.</span><span class="sxs-lookup"><span data-stu-id="48742-122">For example, when using a record set of type 'A', you need to specify the IP address using the parameter `-IPv4Address`.</span></span> <span data-ttu-id="48742-123">Další parametry jsou používány pro jiné typy záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-123">Other parameters are used for other record types.</span></span> <span data-ttu-id="48742-124">V tématu [Příklady dalších typů záznamu](#additional-record-type-examples) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="48742-124">See [Additional record type examples](#additional-record-type-examples) for details.</span></span>

<span data-ttu-id="48742-125">Následující příklad vytvoří záznamů s relativním názvem "www" v zóně DNS "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="48742-125">The following example creates a record set with the relative name 'www' in the DNS Zone 'contoso.com'.</span></span> <span data-ttu-id="48742-126">Plně kvalifikovaný název sady záznamů je "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="48742-126">The fully-qualified name of the record set is 'www.contoso.com'.</span></span> <span data-ttu-id="48742-127">Typ záznamu je "A", a hodnota TTL 3 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="48742-127">The record type is 'A', and the TTL is 3600 seconds.</span></span> <span data-ttu-id="48742-128">Sada záznamů obsahuje jediný záznam, s IP adresou: 1.2.3.4'.</span><span class="sxs-lookup"><span data-stu-id="48742-128">The record set contains a single record, with IP address '1.2.3.4'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="48742-129">Vytvoření záznamu nastaven na 'vrcholu' zónu (v takovém případě "contoso.com"), použijte název sady záznamů "@" (bez uvozovek):</span><span class="sxs-lookup"><span data-stu-id="48742-129">To create a record set at the 'apex' of a zone (in this case, 'contoso.com'), use the record set name '@' (excluding quotation marks):</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="48742-130">Pokud potřebujete vytvořit záznam nastavení, která obsahuje více než jeden záznam, místní pole a je nutné přidat záznamy, vytvořte nejdříve předat pole do `New-AzureRmDnsRecordSet` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="48742-130">If you need to create a record set containing more than one record, first create a local array and add the records, then pass the array to `New-AzureRmDnsRecordSet` as follows:</span></span>

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

<span data-ttu-id="48742-131">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze přidružit každá sada záznamů specifická data jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="48742-131">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="48742-132">Následující příklad ukazuje postup vytvoření záznamů s dvě položky metadat se oddělení = finanční ' a ' prostředí = produkční '.</span><span class="sxs-lookup"><span data-stu-id="48742-132">The following example shows how to create a record set with two metadata entries, 'dept=finance' and 'environment=production'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

<span data-ttu-id="48742-133">Azure DNS podporuje také 'prázdné, sady záznamů, které může fungovat jako zástupný symbol rezervovat název DNS před vytvořením záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-133">Azure DNS also supports 'empty' record sets, which can act as a placeholder to reserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="48742-134">Prázdný sady záznamů jsou viditelné v rovině řízení Azure DNS, ale zobrazí na názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-134">Empty record sets are visible in the Azure DNS control plane, but do appear on the Azure DNS name servers.</span></span> <span data-ttu-id="48742-135">Následující příklad vytvoří prázdná sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="48742-135">The following example creates an empty record set:</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a><span data-ttu-id="48742-136">Vytvořte záznamy jiné typy</span><span class="sxs-lookup"><span data-stu-id="48742-136">Create records of other types</span></span>

<span data-ttu-id="48742-137">S zobrazená v podrobnosti o tom, jak vytvořit záznamy 'A', následující příklady ukazují, jak vytvořit záznamy jiné typy záznamů nepodporuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-137">Having seen in detail how to create 'A' records, the following examples show how to create records of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="48742-138">V každém případě ukážeme, jak vytvořit sadu, která obsahuje jediný záznam záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-138">In each case, we show how to create a record set containing a single record.</span></span> <span data-ttu-id="48742-139">Starší příklady "A" záznamů lze upravit k vytvoření sady záznamů jiných typů obsahující více záznamů s metadaty, nebo k vytvoření sady záznamů o prázdný.</span><span class="sxs-lookup"><span data-stu-id="48742-139">The earlier examples for 'A' records can be adapted to create record sets of other types containing multiple records, with metadata, or to create empty record sets.</span></span>

<span data-ttu-id="48742-140">Jsme neudělujte příklad se vytvořit sadu záznamů SOA, protože se vytvářejí SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="48742-140">We do not give an example to create an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="48742-141">Ale [SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="48742-141">However, [the SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record-set-with-a-single-record"></a><span data-ttu-id="48742-142">Vytvoření sady záznamů AAAA s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-142">Create an AAAA record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a><span data-ttu-id="48742-143">Vytvoření sady záznamů CNAME s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-143">Create a CNAME record set with a single record</span></span>

> [!NOTE]
> <span data-ttu-id="48742-144">Standardech DNS nepovoluje záznamy CNAME na vrcholu zóny (`-Name '@'`), ani se povolit sad záznamů obsahující více než jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="48742-144">The DNS standards do not permit CNAME records at the apex of a zone (`-Name '@'`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="48742-145">Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="48742-145">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a><span data-ttu-id="48742-146">Vytvoření sady záznamů MX s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-146">Create an MX record set with a single record</span></span>

<span data-ttu-id="48742-147">V tomto příkladu používáme název sady záznamů "@" k vytvoření záznamu MX ve vrcholu zóny (v tomto případě "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="48742-147">In this example, we use the record set name '@' to create an MX record at the zone apex (in this case, 'contoso.com').</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a><span data-ttu-id="48742-148">Vytvoření sady záznamů NS s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-148">Create an NS record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a><span data-ttu-id="48742-149">Vytvoření sady záznamů PTR s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-149">Create a PTR record set with a single record</span></span>

<span data-ttu-id="48742-150">V takovém případě se Moje-arpa-server zone.com' představuje zóny zpětného vyhledávání ARPA představující rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="48742-150">In this case, 'my-arpa-zone.com' represents the ARPA reverse lookup zone representing your IP range.</span></span> <span data-ttu-id="48742-151">Každá sada záznamů PTR v této zóně odpovídá IP adrese v rámci tohoto rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="48742-151">Each PTR record set in this zone corresponds to an IP address within this IP range.</span></span> <span data-ttu-id="48742-152">Název záznamu "10" je poslední oktet IP adresy v tomto rozsahu IP reprezentována tento záznam.</span><span class="sxs-lookup"><span data-stu-id="48742-152">The record name '10' is the last octet of the IP address within this IP range represented by this record.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a><span data-ttu-id="48742-153">Vytvoření sady záznamů SRV s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-153">Create an SRV record set with a single record</span></span>

<span data-ttu-id="48742-154">Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte  *\_služby* a  *\_protokol* v názvu sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-154">When creating an [SRV record set](dns-zones-records.md#srv-records), specify the *\_service* and *\_protocol* in the record set name.</span></span> <span data-ttu-id="48742-155">Není nutné zahrnout ' @' v názvu sady záznamů při vytváření záznamu SRV nastavit ve vrcholu zóny.</span><span class="sxs-lookup"><span data-stu-id="48742-155">There is no need to include '@' in the record set name when creating an SRV record set at the zone apex.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a><span data-ttu-id="48742-156">Vytvořit záznam TXT s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="48742-156">Create a TXT record set with a single record</span></span>

<span data-ttu-id="48742-157">Následující příklad ukazuje, jak vytvořit záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="48742-157">The following example shows how to create a TXT record.</span></span> <span data-ttu-id="48742-158">Další informace o maximální délku řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="48742-158">For more information about the maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a><span data-ttu-id="48742-159">Získání sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-159">Get a record set</span></span>

<span data-ttu-id="48742-160">Chcete-li načíst existující sady záznamů, použijte `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="48742-160">To retrieve an existing record set, use `Get-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="48742-161">Tato rutina vrátí místní objekt, který reprezentuje sady záznamů v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-161">This cmdlet returns a local object that represents the record set in Azure DNS.</span></span>

<span data-ttu-id="48742-162">Stejně jako u `New-AzureRmDnsRecordSet`, musí být zadaný název sady záznamů *relativní* název, což znamená, je nutné vyloučit název zóny.</span><span class="sxs-lookup"><span data-stu-id="48742-162">As with `New-AzureRmDnsRecordSet`, the record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span> <span data-ttu-id="48742-163">Budete taky muset zadat typ záznamu a sadu zón, který obsahuje záznam.</span><span class="sxs-lookup"><span data-stu-id="48742-163">You also need to specify the record type, and the zone containing the record set.</span></span>

<span data-ttu-id="48742-164">Následující příklad ukazuje, jak načíst sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-164">The following example shows how to retrieve a record set.</span></span> <span data-ttu-id="48742-165">V tomto příkladu je zadána zóny pomocí `-ZoneName` a `-ResourceGroupName` parametry.</span><span class="sxs-lookup"><span data-stu-id="48742-165">In this example, the zone is specified using the `-ZoneName` and `-ResourceGroupName` parameters.</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="48742-166">Alternativně můžete také určit zóny pomocí objekt zóny, předaným pomocí `-Zone` parametr.</span><span class="sxs-lookup"><span data-stu-id="48742-166">Alternatively, you can also specify the zone using a zone object, passed using the `-Zone` parameter.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a><span data-ttu-id="48742-167">Seznam sad záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-167">List record sets</span></span>

<span data-ttu-id="48742-168">Můžete také použít `Get-AzureRmDnsZone` do seznamu sad záznamů v zóně, díky vynechání `-Name` nebo `-RecordType` parametry.</span><span class="sxs-lookup"><span data-stu-id="48742-168">You can also use `Get-AzureRmDnsZone` to list record sets in a zone, by omitting the `-Name` and/or `-RecordType` parameters.</span></span>

<span data-ttu-id="48742-169">Následující příklad vrací sady všech záznamů v zóně:</span><span class="sxs-lookup"><span data-stu-id="48742-169">The following example returns all record sets in the zone:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="48742-170">Následující příklad ukazuje, jak všechny daného typu sady záznamů s typem záznamu při vynechání záznam sady název se dá načíst:</span><span class="sxs-lookup"><span data-stu-id="48742-170">The following example shows how all record sets of a given type can be retrieved by specifying the record type while omitting the record set name:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="48742-171">Pokud chcete načíst všechny sady záznamů se zadaným názvem mezi typy záznamů, budete muset načíst všechny sady záznamů a poté filtrování výsledků:</span><span class="sxs-lookup"><span data-stu-id="48742-171">To retrieve all record sets with a given name, across record types, you need to retrieve all record sets and then filter the results:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

<span data-ttu-id="48742-172">Ve všech výše uvedených příkladech zóny lze zadat buď pomocí `-ZoneName` a `-ResourceGroupName`parametry (jak je znázorněno), nebo zadáním objekt zóny:</span><span class="sxs-lookup"><span data-stu-id="48742-172">In all the above examples, the zone can be specified either by using the `-ZoneName` and `-ResourceGroupName`parameters (as shown), or by specifying a zone object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a><span data-ttu-id="48742-173">Přidání záznamu do existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-173">Add a record to an existing record set</span></span>

<span data-ttu-id="48742-174">Přidání záznamu do existující sady záznamů, použijte následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="48742-174">To add a record to an existing record set, follow the following three steps:</span></span>

1. <span data-ttu-id="48742-175">Získat existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-175">Get the existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="48742-176">Přidejte nový záznam pro místní sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-176">Add the new record to the local record set.</span></span> <span data-ttu-id="48742-177">Toto je offline operace.</span><span class="sxs-lookup"><span data-stu-id="48742-177">This is an off-line operation.</span></span>

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="48742-178">Potvrďte změnu zpět ke službě Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-178">Commit the change back to the Azure DNS service.</span></span> 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

<span data-ttu-id="48742-179">Pomocí `Set-AzureRmDnsRecordSet` *nahrazuje* existující sady záznamů v Azure DNS (a všechny záznamy, které obsahuje) s zadané sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-179">Using `Set-AzureRmDnsRecordSet` *replaces* the existing record set in Azure DNS (and all records it contains) with the record set specified.</span></span> <span data-ttu-id="48742-180">[Značka Etag kontroly](dns-zones-records.md#etags) zajišťují souběžných změny se nepřepíšou.</span><span class="sxs-lookup"><span data-stu-id="48742-180">[Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="48742-181">Můžete použít nepovinný `-Overwrite` přepínač tak, aby potlačit těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="48742-181">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

<span data-ttu-id="48742-182">Tato posloupnost operací může být také *přesměruje*, což znamená předat objekt sady záznamů pomocí kanálu spíše než předání jako parametr:</span><span class="sxs-lookup"><span data-stu-id="48742-182">This sequence of operations can also be *piped*, meaning you pass the record set object by using the pipe rather than passing it as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="48742-183">Výše uvedené příklady ukazují, jak přidat záznam "A" do existující sady záznamů typu "A".</span><span class="sxs-lookup"><span data-stu-id="48742-183">The examples above show how to add an 'A' record to an existing record set of type 'A'.</span></span> <span data-ttu-id="48742-184">Podobně jako posloupnost operací se používá k přidání záznamů do sad záznamů jiných typů, nahraďte `-Ipv4Address` parametr `Add-AzureRmDnsRecordConfig` s další parametry, které jsou specifické pro každý typ záznamu.</span><span class="sxs-lookup"><span data-stu-id="48742-184">A similar sequence of operations is used to add records to record sets of other types, substituting the `-Ipv4Address` parameter of `Add-AzureRmDnsRecordConfig` with other parameters specific to each record type.</span></span> <span data-ttu-id="48742-185">Parametry pro každý typ záznamu jsou stejné jako u `New-AzureRmDnsRecordConfig` rutiny, jak je znázorněno v [Příklady dalších typů záznamu](#additional-record-type-examples) výše.</span><span class="sxs-lookup"><span data-stu-id="48742-185">The parameters for each record type are the same as for the `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>

<span data-ttu-id="48742-186">Sady záznamů typu 'CNAME' nebo 'SOA, nemůže obsahovat více než jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="48742-186">Record sets of type 'CNAME' or 'SOA' cannot contain more than one record.</span></span> <span data-ttu-id="48742-187">Toto omezení mohou nastat z norem DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-187">This constraint arises from the DNS standards.</span></span> <span data-ttu-id="48742-188">Není omezení Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-188">It is not a limitation of Azure DNS.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="48742-189">Odebrat záznam z existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-189">Remove a record from an existing record set</span></span>

<span data-ttu-id="48742-190">Postup odebrání záznamu ze sady záznamů se podobá postupu přidání záznamu do existující sady záznamů:</span><span class="sxs-lookup"><span data-stu-id="48742-190">The process to remove a record from a record set is similar to the process to add a record to an existing record set:</span></span>

1. <span data-ttu-id="48742-191">Získat existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-191">Get the existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="48742-192">Odeberte záznam z objektu místní sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-192">Remove the record from the local record set object.</span></span> <span data-ttu-id="48742-193">Toto je offline operace.</span><span class="sxs-lookup"><span data-stu-id="48742-193">This is an off-line operation.</span></span> <span data-ttu-id="48742-194">Záznam, který má být odebrán musí být přesně shodovat s existujícím záznamem v rámci všech parametrů.</span><span class="sxs-lookup"><span data-stu-id="48742-194">The record that's being removed must be an exact match with an existing record across all parameters.</span></span>

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="48742-195">Potvrďte změnu zpět ke službě Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-195">Commit the change back to the Azure DNS service.</span></span> <span data-ttu-id="48742-196">Použít nepovinný `-Overwrite` přepínač tak, aby potlačit [kontroluje Etag](dns-zones-records.md#etags) souběžných změny.</span><span class="sxs-lookup"><span data-stu-id="48742-196">Use the optional `-Overwrite` switch to suppress [Etag checks](dns-zones-records.md#etags) for concurrent changes.</span></span>

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

<span data-ttu-id="48742-197">Pomocí výše uvedených pořadí odebrat poslední záznam ze záznamů sady nedojde k odstranění sady záznamů, místo zůstanou prázdná sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-197">Using the above sequence to remove the last record from a record set does not delete the record set, rather it leaves an empty record set.</span></span> <span data-ttu-id="48742-198">Odebrat záznam zcela nastavit, najdete v části [odstranit sadu záznamů](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="48742-198">To remove a record set entirely, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="48742-199">Podobně jako při přidávání záznamů do sady záznamů, pořadí operací odebrat sadu záznamů lze také přesměrovat:</span><span class="sxs-lookup"><span data-stu-id="48742-199">Similarly to adding records to a record set, the sequence of operations to remove a record set can also be piped:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="48742-200">Podporuje předávání vhodné parametry specifický pro různé typy záznamů `Remove-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="48742-200">Different record types are supported by passing the appropriate type-specific parameters to `Remove-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="48742-201">Parametry pro každý typ záznamu jsou stejné jako u `New-AzureRmDnsRecordConfig` rutiny, jak je znázorněno v [Příklady dalších typů záznamu](#additional-record-type-examples) výše.</span><span class="sxs-lookup"><span data-stu-id="48742-201">The parameters for each record type are the same as for the `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>


## <a name="modify-an-existing-record-set"></a><span data-ttu-id="48742-202">Upravit existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-202">Modify an existing record set</span></span>

<span data-ttu-id="48742-203">Postup úpravy existující sady záznamů jsou podobné kroky, které můžete provést při přidávání nebo odebírání záznamů z sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="48742-203">The steps for modifying an existing record set are similar to the steps you take when adding or removing records from a record set:</span></span>

1. <span data-ttu-id="48742-204">Načtení záznamu stávajícího nastavit pomocí `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="48742-204">Retrieve the existing record set by using `Get-AzureRmDnsRecordSet`.</span></span>
2. <span data-ttu-id="48742-205">Upravte objekt místní sady záznamů podle:</span><span class="sxs-lookup"><span data-stu-id="48742-205">Modify the local record set object by:</span></span>
    * <span data-ttu-id="48742-206">Přidání nebo odebrání záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-206">Adding or removing records</span></span>
    * <span data-ttu-id="48742-207">Změna parametrů existující záznamy</span><span class="sxs-lookup"><span data-stu-id="48742-207">Changing the parameters of existing records</span></span>
    * <span data-ttu-id="48742-208">Změna záznamu nastavte metadata a čas pro live (TTL)</span><span class="sxs-lookup"><span data-stu-id="48742-208">Changing the record set metadata and time to live (TTL)</span></span>
3. <span data-ttu-id="48742-209">Potvrdit změny pomocí `Set-AzureRmDnsRecordSet` rutiny.</span><span class="sxs-lookup"><span data-stu-id="48742-209">Commit your changes by using the `Set-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="48742-210">To *nahrazuje* existující sady záznamů v Azure DNS s zadané sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-210">This *replaces* the existing record set in Azure DNS with the record set specified.</span></span>

<span data-ttu-id="48742-211">Při použití `Set-AzureRmDnsRecordSet`, [kontroluje Etag](dns-zones-records.md#etags) zajišťují souběžných změny se nepřepíšou.</span><span class="sxs-lookup"><span data-stu-id="48742-211">When using `Set-AzureRmDnsRecordSet`, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="48742-212">Můžete použít nepovinný `-Overwrite` přepínač tak, aby potlačit těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="48742-212">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

### <a name="to-update-a-record-in-an-existing-record-set"></a><span data-ttu-id="48742-213">Chcete-li aktualizovat záznam, v existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-213">To update a record in an existing record set</span></span>

<span data-ttu-id="48742-214">V tomto příkladu se nám změnit IP adresu existující "A" záznamu:</span><span class="sxs-lookup"><span data-stu-id="48742-214">In this example, we change the IP address of an existing 'A' record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a><span data-ttu-id="48742-215">Úprava záznamu SOA</span><span class="sxs-lookup"><span data-stu-id="48742-215">To modify an SOA record</span></span>

<span data-ttu-id="48742-216">Nelze přidat nebo odebrat záznamy z automaticky vytvořený záznam SOA nastavit ve vrcholu zóny (`-Name "@"`, včetně uvozovek nahoře).</span><span class="sxs-lookup"><span data-stu-id="48742-216">You cannot add or remove records from the automatically created SOA record set at the zone apex (`-Name "@"`, including quote marks).</span></span> <span data-ttu-id="48742-217">Ale můžete upravit některý z parametrů v rámci záznamu SOA (s výjimkou "hostitel") a hodnota TTL sady záznamu.</span><span class="sxs-lookup"><span data-stu-id="48742-217">However, you can modify any of the parameters within the SOA record (except "Host") and the record set TTL.</span></span>

<span data-ttu-id="48742-218">Následující příklad ukazuje, jak změnit *e-mailu* vlastnost záznamu SOA:</span><span class="sxs-lookup"><span data-stu-id="48742-218">The following example shows how to change the *Email* property of the SOA record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="48742-219">Chcete-li upravit NS záznamů ve vrcholu zóny</span><span class="sxs-lookup"><span data-stu-id="48742-219">To modify NS records at the zone apex</span></span>

<span data-ttu-id="48742-220">S každou zónou DNS se automaticky vytvoří záznam NS nastavit ve vrcholu zóny.</span><span class="sxs-lookup"><span data-stu-id="48742-220">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="48742-221">Obsahuje názvy názvových serverů Azure DNS přiřadit do zóny.</span><span class="sxs-lookup"><span data-stu-id="48742-221">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="48742-222">Můžete přidat další název nastavit servery na tento záznam NS, podporuje společné hosting domén s více než jednoho poskytovatele DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-222">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="48742-223">Můžete také upravit TTL a metadat pro tuto sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-223">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="48742-224">Však nelze odebrat ani změnit předem vyplněná názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-224">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="48742-225">Všimněte si, že vztahuje se pouze na vrcholu zóny sady záznamů NS.</span><span class="sxs-lookup"><span data-stu-id="48742-225">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="48742-226">Jiné sady záznamů NS v pásmu (jak je používá delegovat podřízených zónách) můžete změnit bez omezení.</span><span class="sxs-lookup"><span data-stu-id="48742-226">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="48742-227">Následující příklad ukazuje, jak přidat do sady ve vrcholu zóny záznamů NS k další název serveru:</span><span class="sxs-lookup"><span data-stu-id="48742-227">The following example shows how to add an additional name server to the NS record set at the zone apex:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a><span data-ttu-id="48742-228">Chcete-li upravit metadata sady záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-228">To modify record set metadata</span></span>

<span data-ttu-id="48742-229">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze přidružit každá sada záznamů specifická data jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="48742-229">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="48742-230">Následující příklad ukazuje, jak upravit metadata existující sady záznamů:</span><span class="sxs-lookup"><span data-stu-id="48742-230">The following example shows how to modify the metadata of an existing record set:</span></span>

```powershell
# Get the record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a><span data-ttu-id="48742-231">Odstranit sadu záznamů</span><span class="sxs-lookup"><span data-stu-id="48742-231">Delete a record set</span></span>

<span data-ttu-id="48742-232">Sady záznamů lze odstranit pomocí `Remove-AzureRmDnsRecordSet` rutiny.</span><span class="sxs-lookup"><span data-stu-id="48742-232">Record sets can be deleted by using the `Remove-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="48742-233">Odstranění sady záznamů se také odstraní všechny záznamy v sadě záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-233">Deleting a record set also deletes all records within the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="48742-234">Nelze odstranit, SOA a sady záznamů NS ve vrcholu zóny (`-Name '@'`).</span><span class="sxs-lookup"><span data-stu-id="48742-234">You cannot delete the SOA and NS record sets at the zone apex (`-Name '@'`).</span></span>  <span data-ttu-id="48742-235">Azure DNS tyto vytvořena automaticky při zóny byl vytvořen a je při odstranění zóny automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="48742-235">Azure DNS created these automatically when the zone was created, and deletes them automatically when the zone is deleted.</span></span>

<span data-ttu-id="48742-236">Následující příklad ukazuje, jak odstranit sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="48742-236">The following example shows how to delete a record set.</span></span> <span data-ttu-id="48742-237">V tomto příkladu název sady záznamů, typ sady záznamů, název zóny a skupina prostředků se každý zadaný explicitně.</span><span class="sxs-lookup"><span data-stu-id="48742-237">In this example, the record set name, record set type, zone name, and resource group are each specified explicitly.</span></span>

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="48742-238">Sada záznamů můžete alternativně zadat podle názvu a typu a zóny zadán pomocí objektu:</span><span class="sxs-lookup"><span data-stu-id="48742-238">Alternatively, the record set can be specified by name and type, and the zone specified using an object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

<span data-ttu-id="48742-239">Jako pomocí třetí volby, samotné sady záznamů je možné zadat pomocí objekt sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="48742-239">As a third option, the record set itself can be specified using a record set object:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="48742-240">Když zadáte sady k odstranění pomocí objektu sady záznamů, záznamů [kontroluje Etag](dns-zones-records.md#etags) zajišťují souběžných změny nebudou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="48742-240">When you specify the record set to be deleted by using a record set object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not deleted.</span></span> <span data-ttu-id="48742-241">Můžete použít nepovinný `-Overwrite` přepínač tak, aby potlačit těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="48742-241">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

<span data-ttu-id="48742-242">Objekt sady záznamů lze také přesměrovat místo předávány jako parametr:</span><span class="sxs-lookup"><span data-stu-id="48742-242">The record set object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a><span data-ttu-id="48742-243">Potvrzení výzvy</span><span class="sxs-lookup"><span data-stu-id="48742-243">Confirmation prompts</span></span>

<span data-ttu-id="48742-244">`New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, A `Remove-AzureRmDnsRecordSet` rutiny všechny podporují potvrzení výzvy.</span><span class="sxs-lookup"><span data-stu-id="48742-244">The `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, and `Remove-AzureRmDnsRecordSet` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="48742-245">Každá rutina vyzve k potvrzení, pokud `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="48742-245">Each cmdlet prompts for confirmation if the `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="48742-246">Protože výchozí hodnota pro `$ConfirmPreference` je `High`, tyto výzvy nejsou zadána při pomocí výchozích nastavení prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48742-246">Since the default value for `$ConfirmPreference` is `High`, these prompts are not given when using the default PowerShell settings.</span></span>

<span data-ttu-id="48742-247">Můžete přepsat aktuální `$ConfirmPreference` nastavení pomocí `-Confirm` parametr.</span><span class="sxs-lookup"><span data-stu-id="48742-247">You can override the current `$ConfirmPreference` setting using the `-Confirm` parameter.</span></span> <span data-ttu-id="48742-248">Pokud zadáte `-Confirm` nebo `-Confirm:$True` , rutina vás vyzve k potvrzení před jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="48742-248">If you specify `-Confirm` or `-Confirm:$True` , the cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="48742-249">Pokud zadáte `-Confirm:$False` , rutina nezobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="48742-249">If you specify `-Confirm:$False` , the cmdlet does not prompt you for confirmation.</span></span> 

<span data-ttu-id="48742-250">Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="48742-250">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="48742-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48742-251">Next steps</span></span>

<span data-ttu-id="48742-252">Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="48742-252">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="48742-253">Zjistěte, jak [chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="48742-253">Learn how to [protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
<br>
<span data-ttu-id="48742-254">Zkontrolujte [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="48742-254">Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>
