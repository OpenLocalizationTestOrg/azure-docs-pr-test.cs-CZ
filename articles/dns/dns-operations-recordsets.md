---
title: "zaznamenává aaaManage DNS v Azure DNS pomocí Azure PowerShell | Microsoft Docs"
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
ms.openlocfilehash: bfdf116e174d06db0514abdc0ec3f4fc4ee0a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a><span data-ttu-id="0f51a-104">Správa záznamů DNS a sady záznamů v Azure DNS pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0f51a-104">Manage DNS records and recordsets in Azure DNS using Azure PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f51a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0f51a-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="0f51a-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="0f51a-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="0f51a-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="0f51a-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="0f51a-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0f51a-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="0f51a-109">Tento článek ukazuje, jak toomanage DNS záznamy pro zóny DNS pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0f51a-109">This article shows you how toomanage DNS records for your DNS zone by using Azure PowerShell.</span></span> <span data-ttu-id="0f51a-110">Záznamy DNS se taky dají spravovat pomocí hello napříč platformami [rozhraní příkazového řádku Azure](dns-operations-recordsets-cli.md) nebo hello [portál Azure](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0f51a-110">DNS records can also be managed by using hello cross-platform [Azure CLI](dns-operations-recordsets-cli.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

<span data-ttu-id="0f51a-111">Hello příklady v tomto článku předpokládá, že již máte [nainstalovaný Azure PowerShell, přihlášení a vytvořit zónu DNS](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="0f51a-111">hello examples in this article assume you have already [installed Azure PowerShell, signed in, and created a DNS zone](dns-operations-dnszones.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="0f51a-112">Úvod</span><span class="sxs-lookup"><span data-stu-id="0f51a-112">Introduction</span></span>

<span data-ttu-id="0f51a-113">Před vytvořením záznamy DNS v Azure DNS, musíte nejdřív toounderstand jak Azure DNS jsou uspořádány záznamy DNS do sady záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-113">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="0f51a-114">Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="0f51a-114">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>


## <a name="create-a-new-dns-record"></a><span data-ttu-id="0f51a-115">Vytvořit nový záznam DNS</span><span class="sxs-lookup"><span data-stu-id="0f51a-115">Create a new DNS record</span></span>

<span data-ttu-id="0f51a-116">Pokud má nový záznam hello stejný název a zadejte jako stávající záznam, je třeba příliš[ho přidat existující sady záznamů toohello](#add-a-record-to-an-existing-record-set).</span><span class="sxs-lookup"><span data-stu-id="0f51a-116">If your new record has hello same name and type as an existing record, you need too[add it toohello existing record set](#add-a-record-to-an-existing-record-set).</span></span> <span data-ttu-id="0f51a-117">Pokud nový záznam má jiný název a typ tooall, existující záznamy, je třeba toocreate nové sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-117">If your new record has a different name and type tooall existing records, you need toocreate a new record set.</span></span> 

### <a name="create-a-records-in-a-new-record-set"></a><span data-ttu-id="0f51a-118">Vytvořit "A" záznamy v nové sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-118">Create 'A' records in a new record set</span></span>

<span data-ttu-id="0f51a-119">Vytvoření sady záznamů pomocí hello `New-AzureRmDnsRecordSet` rutiny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-119">You create record sets by using hello `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="0f51a-120">Při vytváření sady záznamů, je třeba název sady záznamů hello toospecify, hello zóny, době hello toolive (TTL), typ záznamu hello a hello záznamy toobe vytvoření.</span><span class="sxs-lookup"><span data-stu-id="0f51a-120">When creating a record set, you need toospecify hello record set name, hello zone, hello time toolive (TTL), hello record type, and hello records toobe created.</span></span>

<span data-ttu-id="0f51a-121">Hello parametry pro přidání sady záznamů tooa záznamů se liší v závislosti na typu hello hello sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-121">hello parameters for adding records tooa record set vary depending on hello type of hello record set.</span></span> <span data-ttu-id="0f51a-122">Například pokud používáte sadu záznamů typu "A", budete potřebovat toospecify hello IP adresu pomocí parametru hello `-IPv4Address`.</span><span class="sxs-lookup"><span data-stu-id="0f51a-122">For example, when using a record set of type 'A', you need toospecify hello IP address using hello parameter `-IPv4Address`.</span></span> <span data-ttu-id="0f51a-123">Další parametry jsou používány pro jiné typy záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-123">Other parameters are used for other record types.</span></span> <span data-ttu-id="0f51a-124">V tématu [Příklady dalších typů záznamu](#additional-record-type-examples) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0f51a-124">See [Additional record type examples](#additional-record-type-examples) for details.</span></span>

<span data-ttu-id="0f51a-125">Hello následující příklad vytvoří záznamů s relativním názvem "www" v zóně DNS "contoso.com" hello hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-125">hello following example creates a record set with hello relative name 'www' in hello DNS Zone 'contoso.com'.</span></span> <span data-ttu-id="0f51a-126">Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="0f51a-126">hello fully-qualified name of hello record set is 'www.contoso.com'.</span></span> <span data-ttu-id="0f51a-127">Typ záznamu Hello je "A" a je hello TTL 3 600 sekund.</span><span class="sxs-lookup"><span data-stu-id="0f51a-127">hello record type is 'A', and hello TTL is 3600 seconds.</span></span> <span data-ttu-id="0f51a-128">Hello sada záznamů obsahuje jediný záznam, s IP adresou: 1.2.3.4'.</span><span class="sxs-lookup"><span data-stu-id="0f51a-128">hello record set contains a single record, with IP address '1.2.3.4'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="0f51a-129">toocreate sada záznamů v hello vrcholu zóny (v tomto případě "contoso.com"), použijte název sady záznamů hello ' @' (bez uvozovek):</span><span class="sxs-lookup"><span data-stu-id="0f51a-129">toocreate a record set at hello 'apex' of a zone (in this case, 'contoso.com'), use hello record set name '@' (excluding quotation marks):</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="0f51a-130">Pokud potřebujete toocreate sada záznamů obsahující více než jeden záznam, místní pole a přidat záznamy hello vytvořte nejdříve předat hello pole příliš`New-AzureRmDnsRecordSet` následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="0f51a-130">If you need toocreate a record set containing more than one record, first create a local array and add hello records, then pass hello array too`New-AzureRmDnsRecordSet` as follows:</span></span>

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

<span data-ttu-id="0f51a-131">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="0f51a-131">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="0f51a-132">Hello následující příklad ukazuje, jak toocreate sada záznamů s dvě položky metadat se oddělení = finanční ' a ' prostředí = produkční '.</span><span class="sxs-lookup"><span data-stu-id="0f51a-132">hello following example shows how toocreate a record set with two metadata entries, 'dept=finance' and 'environment=production'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

<span data-ttu-id="0f51a-133">Azure DNS podporuje také 'prázdné, sady záznamů, které může fungovat jako zástupný symbol tooreserve název DNS před vytvořením záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-133">Azure DNS also supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="0f51a-134">Prázdný sady záznamů jsou viditelné v rovině řízení hello Azure DNS, ale zobrazí na názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-134">Empty record sets are visible in hello Azure DNS control plane, but do appear on hello Azure DNS name servers.</span></span> <span data-ttu-id="0f51a-135">Hello následující ukázka vytvoří prázdná sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="0f51a-135">hello following example creates an empty record set:</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a><span data-ttu-id="0f51a-136">Vytvořte záznamy jiné typy</span><span class="sxs-lookup"><span data-stu-id="0f51a-136">Create records of other types</span></span>

<span data-ttu-id="0f51a-137">S vidět podrobně jak toocreate "A" záznamy, hello následující příklady zobrazují jak záznam toocreate záznamy jiné typy podporované systémem Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-137">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate records of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="0f51a-138">V každém případě ukážeme, jak nastavit toocreate záznam obsahující jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="0f51a-138">In each case, we show how toocreate a record set containing a single record.</span></span> <span data-ttu-id="0f51a-139">Hello starší příklady záznamy 'A' může být přizpůsobena toocreate sady záznamů jiných typů obsahující více záznamů s metadaty, nebo sady záznamů toocreate prázdný.</span><span class="sxs-lookup"><span data-stu-id="0f51a-139">hello earlier examples for 'A' records can be adapted toocreate record sets of other types containing multiple records, with metadata, or toocreate empty record sets.</span></span>

<span data-ttu-id="0f51a-140">Jsme neudělujte toocreate příklad sady záznamů SOA vzhledem k tomu, že se vytvoří SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="0f51a-140">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="0f51a-141">Ale [hello SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="0f51a-141">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-142">Vytvoření sady záznamů AAAA s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-142">Create an AAAA record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-143">Vytvoření sady záznamů CNAME s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-143">Create a CNAME record set with a single record</span></span>

> [!NOTE]
> <span data-ttu-id="0f51a-144">standardech DNS Hello nepovoluje záznamy CNAME v hello vrcholu zóny (`-Name '@'`), ani se povolit sad záznamů obsahující více než jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="0f51a-144">hello DNS standards do not permit CNAME records at hello apex of a zone (`-Name '@'`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="0f51a-145">Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="0f51a-145">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-146">Vytvoření sady záznamů MX s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-146">Create an MX record set with a single record</span></span>

<span data-ttu-id="0f51a-147">V tomto příkladu používáme hello název sady záznamů "@" záznam toocreate MX ve vrcholu zóny hello (v tomto případě "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="0f51a-147">In this example, we use hello record set name '@' toocreate an MX record at hello zone apex (in this case, 'contoso.com').</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-148">Vytvoření sady záznamů NS s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-148">Create an NS record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-149">Vytvoření sady záznamů PTR s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-149">Create a PTR record set with a single record</span></span>

<span data-ttu-id="0f51a-150">V takovém případě se Moje-arpa-server zone.com' představuje hello zóny zpětného vyhledávání ARPA představující rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="0f51a-150">In this case, 'my-arpa-zone.com' represents hello ARPA reverse lookup zone representing your IP range.</span></span> <span data-ttu-id="0f51a-151">Každý záznam PTR nastavení v této zóně odpovídá tooan IP adresu do tohoto rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="0f51a-151">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span> <span data-ttu-id="0f51a-152">název záznamu Hello "10" je poslední oktet hello hello IP adresy v tomto rozsahu IP reprezentována tento záznam.</span><span class="sxs-lookup"><span data-stu-id="0f51a-152">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-153">Vytvoření sady záznamů SRV s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-153">Create an SRV record set with a single record</span></span>

<span data-ttu-id="0f51a-154">Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte hello  *\_služby* a  *\_protokol* v hello název sady záznamu.</span><span class="sxs-lookup"><span data-stu-id="0f51a-154">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="0f51a-155">Neexistuje žádné tooinclude nutné ' @' v hello název sady záznamu při vytváření záznamu SRV nastavit na vrcholu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-155">There is no need tooinclude '@' in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a><span data-ttu-id="0f51a-156">Vytvořit záznam TXT s jedním záznamem</span><span class="sxs-lookup"><span data-stu-id="0f51a-156">Create a TXT record set with a single record</span></span>

<span data-ttu-id="0f51a-157">Hello následující příklad ukazuje, jak záznam toocreate TXT.</span><span class="sxs-lookup"><span data-stu-id="0f51a-157">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="0f51a-158">Další informace o hello maximální délka řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="0f51a-158">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a><span data-ttu-id="0f51a-159">Získání sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-159">Get a record set</span></span>

<span data-ttu-id="0f51a-160">tooretrieve existující sady záznamů, použijte `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="0f51a-160">tooretrieve an existing record set, use `Get-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="0f51a-161">Tato rutina vrátí místní objekt, který reprezentuje hello sady záznamů v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-161">This cmdlet returns a local object that represents hello record set in Azure DNS.</span></span>

<span data-ttu-id="0f51a-162">Stejně jako u `New-AzureRmDnsRecordSet`, musí být zadaný název sady záznamů hello *relativní* název, což znamená, je nutné vyloučit hello název zóny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-162">As with `New-AzureRmDnsRecordSet`, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="0f51a-163">Musíte také typ záznamu hello toospecify a časového pásma hello obsahující sadu záznamů hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-163">You also need toospecify hello record type, and hello zone containing hello record set.</span></span>

<span data-ttu-id="0f51a-164">Hello následující příklad ukazuje, jak tooretrieve sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-164">hello following example shows how tooretrieve a record set.</span></span> <span data-ttu-id="0f51a-165">V tomto příkladu je zadána hello zóna pomocí hello `-ZoneName` a `-ResourceGroupName` parametry.</span><span class="sxs-lookup"><span data-stu-id="0f51a-165">In this example, hello zone is specified using hello `-ZoneName` and `-ResourceGroupName` parameters.</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="0f51a-166">Alternativně můžete také určit hello zóny pomocí objekt zóny, předaným pomocí hello `-Zone` parametr.</span><span class="sxs-lookup"><span data-stu-id="0f51a-166">Alternatively, you can also specify hello zone using a zone object, passed using hello `-Zone` parameter.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a><span data-ttu-id="0f51a-167">Seznam sad záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-167">List record sets</span></span>

<span data-ttu-id="0f51a-168">Můžete také použít `Get-AzureRmDnsZone` sady toolist záznamů v zóně, díky vynechání hello `-Name` nebo `-RecordType` parametry.</span><span class="sxs-lookup"><span data-stu-id="0f51a-168">You can also use `Get-AzureRmDnsZone` toolist record sets in a zone, by omitting hello `-Name` and/or `-RecordType` parameters.</span></span>

<span data-ttu-id="0f51a-169">Hello následující příklad vrací sady všech záznamů v zóně hello:</span><span class="sxs-lookup"><span data-stu-id="0f51a-169">hello following example returns all record sets in hello zone:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="0f51a-170">Hello následující příklad ukazuje, jak všechny sady daného typu záznamů můžete načíst tak, že zadáte typ záznamu hello při vynechání hello záznam sady název:</span><span class="sxs-lookup"><span data-stu-id="0f51a-170">hello following example shows how all record sets of a given type can be retrieved by specifying hello record type while omitting hello record set name:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="0f51a-171">tooretrieve sady všech záznamů se zadaným názvem mezi typy záznamů, budete potřebovat tooretrieve všechny sady záznamů a pak výsledky hello filtru:</span><span class="sxs-lookup"><span data-stu-id="0f51a-171">tooretrieve all record sets with a given name, across record types, you need tooretrieve all record sets and then filter hello results:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

<span data-ttu-id="0f51a-172">Ve všech hello výše příklady hello zóna může být zadán buď pomocí hello `-ZoneName` a `-ResourceGroupName`parametry (jak je znázorněno), nebo zadáním objekt zóny:</span><span class="sxs-lookup"><span data-stu-id="0f51a-172">In all hello above examples, hello zone can be specified either by using hello `-ZoneName` and `-ResourceGroupName`parameters (as shown), or by specifying a zone object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="0f51a-173">Přidání záznamů tooan existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-173">Add a record tooan existing record set</span></span>

<span data-ttu-id="0f51a-174">tooadd stávající záznam záznamů tooan nastavit, postupujte podle hello následující tři kroky:</span><span class="sxs-lookup"><span data-stu-id="0f51a-174">tooadd a record tooan existing record set, follow hello following three steps:</span></span>

1. <span data-ttu-id="0f51a-175">Získat hello existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-175">Get hello existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="0f51a-176">Přidejte hello nového záznamu toohello místní sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-176">Add hello new record toohello local record set.</span></span> <span data-ttu-id="0f51a-177">Toto je offline operace.</span><span class="sxs-lookup"><span data-stu-id="0f51a-177">This is an off-line operation.</span></span>

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="0f51a-178">Potvrďte hello změnu zpět toohello služba Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-178">Commit hello change back toohello Azure DNS service.</span></span> 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

<span data-ttu-id="0f51a-179">Pomocí `Set-AzureRmDnsRecordSet` *nahrazuje* hello existující sady záznamů v Azure DNS (a všechny záznamy, které obsahuje) s sady záznamů hello zadán.</span><span class="sxs-lookup"><span data-stu-id="0f51a-179">Using `Set-AzureRmDnsRecordSet` *replaces* hello existing record set in Azure DNS (and all records it contains) with hello record set specified.</span></span> <span data-ttu-id="0f51a-180">[Značka Etag kontroly](dns-zones-records.md#etags) používají tooensure souběžných změny se nepřepíšou.</span><span class="sxs-lookup"><span data-stu-id="0f51a-180">[Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="0f51a-181">Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="0f51a-181">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

<span data-ttu-id="0f51a-182">Tato posloupnost operací může být také *přesměruje*, což znamená předat objekt sady záznamů hello pomocí kanálu hello spíše než předání jako parametr:</span><span class="sxs-lookup"><span data-stu-id="0f51a-182">This sequence of operations can also be *piped*, meaning you pass hello record set object by using hello pipe rather than passing it as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="0f51a-183">Příklady Hello výše ukazují, jak nastavit tooadd "A" záznamu tooan existujícího záznamu typu "A".</span><span class="sxs-lookup"><span data-stu-id="0f51a-183">hello examples above show how tooadd an 'A' record tooan existing record set of type 'A'.</span></span> <span data-ttu-id="0f51a-184">Podobně jako posloupnost operací je toorecord sad záznamů použité tooadd jiných typů, nahraďte hello `-Ipv4Address` parametr `Add-AzureRmDnsRecordConfig` s jiným typem záznamu konkrétní tooeach parametry.</span><span class="sxs-lookup"><span data-stu-id="0f51a-184">A similar sequence of operations is used tooadd records toorecord sets of other types, substituting hello `-Ipv4Address` parameter of `Add-AzureRmDnsRecordConfig` with other parameters specific tooeach record type.</span></span> <span data-ttu-id="0f51a-185">Hello parametry pro každý typ záznamu jsou hello stejné jako u hello `New-AzureRmDnsRecordConfig` rutiny, jak je znázorněno v [Příklady dalších typů záznamu](#additional-record-type-examples) výše.</span><span class="sxs-lookup"><span data-stu-id="0f51a-185">hello parameters for each record type are hello same as for hello `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>

<span data-ttu-id="0f51a-186">Sady záznamů typu 'CNAME' nebo 'SOA, nemůže obsahovat více než jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="0f51a-186">Record sets of type 'CNAME' or 'SOA' cannot contain more than one record.</span></span> <span data-ttu-id="0f51a-187">Toto omezení vyplývá z norem DNS hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-187">This constraint arises from hello DNS standards.</span></span> <span data-ttu-id="0f51a-188">Není omezení Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-188">It is not a limitation of Azure DNS.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="0f51a-189">Odebrat záznam z existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-189">Remove a record from an existing record set</span></span>

<span data-ttu-id="0f51a-190">Hello tooremove procesu vytvoří záznam na sadu záznamů je podobný procesu tooadd toohello záznamů tooan existující sady záznamů:</span><span class="sxs-lookup"><span data-stu-id="0f51a-190">hello process tooremove a record from a record set is similar toohello process tooadd a record tooan existing record set:</span></span>

1. <span data-ttu-id="0f51a-191">Získat hello existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-191">Get hello existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="0f51a-192">Odeberte záznam hello z objektu hello místní sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-192">Remove hello record from hello local record set object.</span></span> <span data-ttu-id="0f51a-193">Toto je offline operace.</span><span class="sxs-lookup"><span data-stu-id="0f51a-193">This is an off-line operation.</span></span> <span data-ttu-id="0f51a-194">Hello záznam, který má být odebrán musí být přesně shodovat s existujícím záznamem v rámci všech parametrů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-194">hello record that's being removed must be an exact match with an existing record across all parameters.</span></span>

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="0f51a-195">Potvrďte hello změnu zpět toohello služba Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-195">Commit hello change back toohello Azure DNS service.</span></span> <span data-ttu-id="0f51a-196">Volitelné použití hello `-Overwrite` přepínač toosuppress [kontroluje Etag](dns-zones-records.md#etags) souběžných změny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-196">Use hello optional `-Overwrite` switch toosuppress [Etag checks](dns-zones-records.md#etags) for concurrent changes.</span></span>

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

<span data-ttu-id="0f51a-197">Pomocí hello výše pořadí tooremove hello poslední záznam ze záznamů sady neodstraní hello sady záznamů, místo zůstanou prázdná sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-197">Using hello above sequence tooremove hello last record from a record set does not delete hello record set, rather it leaves an empty record set.</span></span> <span data-ttu-id="0f51a-198">tooremove sada záznamů zcela, najdete v části [odstranit sadu záznamů](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="0f51a-198">tooremove a record set entirely, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="0f51a-199">Podobně tooadding záznamy tooa sady záznamů, hello posloupnost operací tooremove, které lze předat také sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="0f51a-199">Similarly tooadding records tooa record set, hello sequence of operations tooremove a record set can also be piped:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="0f51a-200">Různé typy záznamů jsou podporovány předáním příslušné parametry specifické pro typ hello příliš`Remove-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="0f51a-200">Different record types are supported by passing hello appropriate type-specific parameters too`Remove-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="0f51a-201">Hello parametry pro každý typ záznamu jsou hello stejné jako u hello `New-AzureRmDnsRecordConfig` rutiny, jak je znázorněno v [Příklady dalších typů záznamu](#additional-record-type-examples) výše.</span><span class="sxs-lookup"><span data-stu-id="0f51a-201">hello parameters for each record type are hello same as for hello `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>


## <a name="modify-an-existing-record-set"></a><span data-ttu-id="0f51a-202">Upravit existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-202">Modify an existing record set</span></span>

<span data-ttu-id="0f51a-203">kroky Hello úpravy existující sady záznamů jsou podobné toohello kroky, jak při přidávání nebo odebírání záznamů z sada záznamů:</span><span class="sxs-lookup"><span data-stu-id="0f51a-203">hello steps for modifying an existing record set are similar toohello steps you take when adding or removing records from a record set:</span></span>

1. <span data-ttu-id="0f51a-204">Načtení záznamu stávajícího hello nastavit pomocí `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="0f51a-204">Retrieve hello existing record set by using `Get-AzureRmDnsRecordSet`.</span></span>
2. <span data-ttu-id="0f51a-205">Upravte objekt hello místní sady záznamů podle:</span><span class="sxs-lookup"><span data-stu-id="0f51a-205">Modify hello local record set object by:</span></span>
    * <span data-ttu-id="0f51a-206">Přidání nebo odebrání záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-206">Adding or removing records</span></span>
    * <span data-ttu-id="0f51a-207">Změna parametrů hello existující záznamy</span><span class="sxs-lookup"><span data-stu-id="0f51a-207">Changing hello parameters of existing records</span></span>
    * <span data-ttu-id="0f51a-208">Změna záznamu hello nastavte metadata a čas toolive (TTL)</span><span class="sxs-lookup"><span data-stu-id="0f51a-208">Changing hello record set metadata and time toolive (TTL)</span></span>
3. <span data-ttu-id="0f51a-209">Potvrdit změny pomocí hello `Set-AzureRmDnsRecordSet` rutiny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-209">Commit your changes by using hello `Set-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="0f51a-210">To *nahrazuje* hello existující sady záznamů v Azure DNS s sady záznamů hello zadán.</span><span class="sxs-lookup"><span data-stu-id="0f51a-210">This *replaces* hello existing record set in Azure DNS with hello record set specified.</span></span>

<span data-ttu-id="0f51a-211">Při použití `Set-AzureRmDnsRecordSet`, [kontroluje Etag](dns-zones-records.md#etags) používají tooensure souběžných změny se nepřepíšou.</span><span class="sxs-lookup"><span data-stu-id="0f51a-211">When using `Set-AzureRmDnsRecordSet`, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="0f51a-212">Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="0f51a-212">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

### <a name="tooupdate-a-record-in-an-existing-record-set"></a><span data-ttu-id="0f51a-213">nastavit tooupdate záznam do stávajícího záznamu</span><span class="sxs-lookup"><span data-stu-id="0f51a-213">tooupdate a record in an existing record set</span></span>

<span data-ttu-id="0f51a-214">V tomto příkladu Změníme hello IP adresu existující "A" záznamu:</span><span class="sxs-lookup"><span data-stu-id="0f51a-214">In this example, we change hello IP address of an existing 'A' record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="0f51a-215">toomodify záznamu SOA</span><span class="sxs-lookup"><span data-stu-id="0f51a-215">toomodify an SOA record</span></span>

<span data-ttu-id="0f51a-216">Nelze přidat nebo odebrat záznamy z hello automaticky vytvořil záznam SOA nastavit na vrcholu zóny hello (`-Name "@"`, včetně uvozovek nahoře).</span><span class="sxs-lookup"><span data-stu-id="0f51a-216">You cannot add or remove records from hello automatically created SOA record set at hello zone apex (`-Name "@"`, including quote marks).</span></span> <span data-ttu-id="0f51a-217">Ale můžete upravit některý z parametrů hello v rámci hello záznam SOA (s výjimkou "hostitel") a záznam hello nastavit hodnotu TTL.</span><span class="sxs-lookup"><span data-stu-id="0f51a-217">However, you can modify any of hello parameters within hello SOA record (except "Host") and hello record set TTL.</span></span>

<span data-ttu-id="0f51a-218">Následující příklad ukazuje, jak Hello toochange hello *e-mailu* vlastnost hello záznam SOA:</span><span class="sxs-lookup"><span data-stu-id="0f51a-218">hello following example shows how toochange hello *Email* property of hello SOA record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="0f51a-219">toomodify NS záznamy na vrcholu zóny hello</span><span class="sxs-lookup"><span data-stu-id="0f51a-219">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="0f51a-220">Nastavte na vrcholu zóny hello záznam NS Hello se vytvoří automaticky s každou zónou DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-220">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="0f51a-221">Obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-221">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="0f51a-222">Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény.</span><span class="sxs-lookup"><span data-stu-id="0f51a-222">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="0f51a-223">Můžete také upravit hello TTL a metadat pro tuto sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-223">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="0f51a-224">Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-224">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="0f51a-225">Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-225">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="0f51a-226">Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) můžete změnit bez omezení.</span><span class="sxs-lookup"><span data-stu-id="0f51a-226">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="0f51a-227">Hello následující příklad ukazuje, jak tooadd záznam NS toohello další název serveru nastavit na vrcholu zóny hello:</span><span class="sxs-lookup"><span data-stu-id="0f51a-227">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-record-set-metadata"></a><span data-ttu-id="0f51a-228">sady záznamů toomodify metadat</span><span class="sxs-lookup"><span data-stu-id="0f51a-228">toomodify record set metadata</span></span>

<span data-ttu-id="0f51a-229">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="0f51a-229">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="0f51a-230">Hello následující příklad ukazuje, jak nastavit toomodify hello metadata existujícího záznamu:</span><span class="sxs-lookup"><span data-stu-id="0f51a-230">hello following example shows how toomodify hello metadata of an existing record set:</span></span>

```powershell
# Get hello record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a><span data-ttu-id="0f51a-231">Odstranit sadu záznamů</span><span class="sxs-lookup"><span data-stu-id="0f51a-231">Delete a record set</span></span>

<span data-ttu-id="0f51a-232">Sady záznamů lze odstranit pomocí hello `Remove-AzureRmDnsRecordSet` rutiny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-232">Record sets can be deleted by using hello `Remove-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="0f51a-233">Odstranění sady záznamů také odstraní všechny záznamy v sadě záznamů hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-233">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="0f51a-234">Nelze odstranit hello SOA a NS sady záznamů na vrcholu zóny hello (`-Name '@'`).</span><span class="sxs-lookup"><span data-stu-id="0f51a-234">You cannot delete hello SOA and NS record sets at hello zone apex (`-Name '@'`).</span></span>  <span data-ttu-id="0f51a-235">Azure DNS tyto vytvořena automaticky při hello zóny byl vytvořen a je při odstranění hello zóny automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="0f51a-235">Azure DNS created these automatically when hello zone was created, and deletes them automatically when hello zone is deleted.</span></span>

<span data-ttu-id="0f51a-236">Hello následující příklad ukazuje, jak toodelete sada záznamů.</span><span class="sxs-lookup"><span data-stu-id="0f51a-236">hello following example shows how toodelete a record set.</span></span> <span data-ttu-id="0f51a-237">V tomto příkladu hello název sady záznamů, typ sady záznamů, název zóny a skupina prostředků se každý zadaný explicitně.</span><span class="sxs-lookup"><span data-stu-id="0f51a-237">In this example, hello record set name, record set type, zone name, and resource group are each specified explicitly.</span></span>

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="0f51a-238">Můžete taky sadu záznamů hello lze zadat pomocí názvu a typu a hello zónu pomocí zadaného objektu:</span><span class="sxs-lookup"><span data-stu-id="0f51a-238">Alternatively, hello record set can be specified by name and type, and hello zone specified using an object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

<span data-ttu-id="0f51a-239">Jako třetí možnost je možné zadat samotné sady záznamů hello pomocí objekt sady záznamů:</span><span class="sxs-lookup"><span data-stu-id="0f51a-239">As a third option, hello record set itself can be specified using a record set object:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="0f51a-240">Když zadáte sady záznamů hello toobe odstranit pomocí objekt sady záznamů, [Etag kontroluje](dns-zones-records.md#etags) používají tooensure souběžných změny nebudou odstraněny.</span><span class="sxs-lookup"><span data-stu-id="0f51a-240">When you specify hello record set toobe deleted by using a record set object, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not deleted.</span></span> <span data-ttu-id="0f51a-241">Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.</span><span class="sxs-lookup"><span data-stu-id="0f51a-241">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

<span data-ttu-id="0f51a-242">objekt sady záznamů Hello lze také přesměrovat místo předávány jako parametr:</span><span class="sxs-lookup"><span data-stu-id="0f51a-242">hello record set object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a><span data-ttu-id="0f51a-243">Potvrzení výzvy</span><span class="sxs-lookup"><span data-stu-id="0f51a-243">Confirmation prompts</span></span>

<span data-ttu-id="0f51a-244">Hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, a `Remove-AzureRmDnsRecordSet` rutiny všechny podporují potvrzení výzvy.</span><span class="sxs-lookup"><span data-stu-id="0f51a-244">hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, and `Remove-AzureRmDnsRecordSet` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="0f51a-245">Každá rutina vyzve k potvrzení, pokud hello `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší.</span><span class="sxs-lookup"><span data-stu-id="0f51a-245">Each cmdlet prompts for confirmation if hello `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="0f51a-246">Od hello výchozí hodnota pro `$ConfirmPreference` je `High`, tyto výzvy nejsou zadána při prostředí PowerShell výchozími nastaveními hello.</span><span class="sxs-lookup"><span data-stu-id="0f51a-246">Since hello default value for `$ConfirmPreference` is `High`, these prompts are not given when using hello default PowerShell settings.</span></span>

<span data-ttu-id="0f51a-247">Můžete přepsat hello aktuální `$ConfirmPreference` nastavení pomocí hello `-Confirm` parametr.</span><span class="sxs-lookup"><span data-stu-id="0f51a-247">You can override hello current `$ConfirmPreference` setting using hello `-Confirm` parameter.</span></span> <span data-ttu-id="0f51a-248">Pokud zadáte `-Confirm` nebo `-Confirm:$True` , hello rutina vás vyzve k potvrzení před jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="0f51a-248">If you specify `-Confirm` or `-Confirm:$True` , hello cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="0f51a-249">Pokud zadáte `-Confirm:$False` , rutina hello nezobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="0f51a-249">If you specify `-Confirm:$False` , hello cmdlet does not prompt you for confirmation.</span></span> 

<span data-ttu-id="0f51a-250">Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="0f51a-250">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f51a-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0f51a-251">Next steps</span></span>

<span data-ttu-id="0f51a-252">Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="0f51a-252">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="0f51a-253">Zjistěte, jak příliš[chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="0f51a-253">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
<br>
<span data-ttu-id="0f51a-254">Zkontrolujte hello [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="0f51a-254">Review hello [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>
