---
title: "Správa záznamů DNS v Azure DNS pomocí Azure CLI 2.0 | Microsoft Docs"
description: "Správa sad záznamů DNS a záznamy v Azure DNS při hostování vaší domény ve službě Azure DNS. Všechny příkazy rozhraní příkazového řádku 2.0 pro operace na sady záznamů a záznamy."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: jonatul
ms.openlocfilehash: 9543759d7ba88c7c5068021cebbeec6b8d63633e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-the-azure-cli-20"></a><span data-ttu-id="4d7e9-104">Správa záznamů DNS a sady záznamů v Azure DNS pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4d7e9-104">Manage DNS records and recordsets in Azure DNS using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d7e9-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4d7e9-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="4d7e9-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="4d7e9-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="4d7e9-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="4d7e9-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="4d7e9-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4d7e9-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="4d7e9-109">Tento článek ukazuje, jak spravovat záznamy DNS pro zónu DNS s použitím různé platformy Azure rozhraní příkazového řádku (CLI) 2.0, která je k dispozici pro Windows, Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-109">This article shows you how to manage DNS records for your DNS zone by using the cross-platform Azure command-line interface (CLI) 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="4d7e9-110">Můžete také spravovat svoje záznamy DNS pomocí [prostředí Azure PowerShell](dns-operations-recordsets.md) nebo [portál Azure](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or the [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="4d7e9-111">Verze rozhraní příkazového řádku pro dokončení úlohy</span><span class="sxs-lookup"><span data-stu-id="4d7e9-111">CLI versions to complete the task</span></span>

<span data-ttu-id="4d7e9-112">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="4d7e9-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) – naše rozhraní příkazového řádku pro klasické modely nasazení a modely nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="4d7e9-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="4d7e9-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) – naše rozhraní příkazového řádku nové generace pro model nasazení správy prostředků</span><span class="sxs-lookup"><span data-stu-id="4d7e9-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

<span data-ttu-id="4d7e9-115">V příkladech v tomto článku předpokládá, že už máte [nainstalovat Azure CLI 2.0, přihlášení a vytvořit zónu DNS](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-115">The examples in this article assume you have already [installed the Azure CLI 2.0, signed in, and created a DNS zone](dns-operations-dnszones-cli.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="4d7e9-116">Úvod</span><span class="sxs-lookup"><span data-stu-id="4d7e9-116">Introduction</span></span>

<span data-ttu-id="4d7e9-117">Před vytvářením záznamů DNS v DNS Azure je nejprve nutné pochopit, jak DNS Azure organizuje záznamy DNS v sadách záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-117">Before creating DNS records in Azure DNS, you first need to understand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="4d7e9-118">Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="4d7e9-119">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="4d7e9-119">Create a DNS record</span></span>

<span data-ttu-id="4d7e9-120">Chcete-li vytvořit záznam DNS, použijte `az network dns record-set <record-type> set-record` příkaz (kde `<record-type>` je typ záznamu, jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="4d7e9-120">To create a DNS record, use the `az network dns record-set <record-type> set-record` command (where `<record-type>` is the type of record, i.e</span></span> <span data-ttu-id="4d7e9-121">a, srv, txt, atd.) Nápovědu získáte příkazem `az network dns record-set --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-121">a, srv, txt, etc.) For help, see `az network dns record-set --help`.</span></span>

<span data-ttu-id="4d7e9-122">Při vytváření záznamu je třeba zadat název skupiny prostředků, název zóny, název sady záznamů a podrobnosti o vytvářeném záznamu.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-122">When creating a record, you need to specify the resource group name, zone name, record set name, the record type, and the details of the record being created.</span></span> <span data-ttu-id="4d7e9-123">Musí být zadaný název sady záznamů *relativní* název, což znamená, je nutné vyloučit název zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-123">The record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span>

<span data-ttu-id="4d7e9-124">Pokud sada záznamů ještě neexistuje, tento příkaz ji vytvoří.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-124">If the record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="4d7e9-125">Pokud sada záznamů již existuje, tento příkaz přidá zadaný záznam do existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-125">If the record set already exists, this command adds the record you specify to the existing record set.</span></span>

<span data-ttu-id="4d7e9-126">Pokud se vytváří nová sada záznamů, použije se výchozí hodnota TTL (Time to Live) 3 600.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="4d7e9-127">Pokyny týkající se používání různých TTLs najdete v tématu [vytvoření sady záznamů DNS](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-127">For instructions on how to use different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="4d7e9-128">Následující příklad vytvoří záznam A s názvem *www* v zóně *contoso.com* ve skupině prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-128">The following example creates an A record called *www* in the zone *contoso.com* in the resource group *MyResourceGroup*.</span></span> <span data-ttu-id="4d7e9-129">IP adresa záznamu A je *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-129">The IP address of the A record is *1.2.3.4*.</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="4d7e9-130">Pokud chcete vytvořit sadu záznamů na vrcholu zóny (v tomto případě „contoso.com“), použijte název záznamu "@" včetně uvozovek:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-130">To create a record set in the apex of the zone (in this case, "contoso.com"), use the record name "@", including the quotation marks:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="4d7e9-131">Vytvoření sady záznamů DNS</span><span class="sxs-lookup"><span data-stu-id="4d7e9-131">Create a DNS record set</span></span>

<span data-ttu-id="4d7e9-132">V uvedených příkladech záznam DNS buď přidala do existující sady záznamů, nebo bylo vytvořeno sady záznamů *implicitně*.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-132">In the above examples, the DNS record was either added to an existing record set, or the record set was created *implicitly*.</span></span> <span data-ttu-id="4d7e9-133">Můžete také vytvořit sadu záznamů *explicitně* před přidání záznamů do ní.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-133">You can also create the record set *explicitly* before adding records to it.</span></span> <span data-ttu-id="4d7e9-134">Azure DNS podporuje 'prázdné, sady záznamů, které může fungovat jako zástupný symbol rezervovat název DNS před vytvořením záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-134">Azure DNS supports 'empty' record sets, which can act as a placeholder to reserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="4d7e9-135">Prázdný sady záznamů jsou viditelné v rovině řízení Azure DNS, ale nejsou zobrazeny na názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-135">Empty record sets are visible in the Azure DNS control plane, but do not appear on the Azure DNS name servers.</span></span>

<span data-ttu-id="4d7e9-136">Sady záznamů jsou vytvořené pomocí `az network dns record-set <record-type> create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-136">Record sets are created using the `az network dns record-set <record-type> create` command.</span></span> <span data-ttu-id="4d7e9-137">Nápovědu získáte příkazem `az network dns record-set <record-type> create --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-137">For help, see `az network dns record-set <record-type> create --help`.</span></span>

<span data-ttu-id="4d7e9-138">Vytvoření záznamu explicitně nastaven umožňuje určit vlastnosti sady záznamů, jako [Time To Live (TTL)](dns-zones-records.md#time-to-live) a metadata.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-138">Creating the record set explicitly allows you to specify record set properties such as the [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="4d7e9-139">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze přidružit každá sada záznamů specifická data jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="4d7e9-140">Následující příklad vytvoří prázdnou sadu záznamů typu "A" s hodnotou TTL 60 sekund, pomocí `--ttl` parametr (krátký tvar `-l`):</span><span class="sxs-lookup"><span data-stu-id="4d7e9-140">The following example creates an empty record set of type 'A' with a 60-second TTL, by using the `--ttl` parameter (short form `-l`):</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

<span data-ttu-id="4d7e9-141">Následující příklad vytvoří záznamů s dvě položky metadat "oddělení = finanční" a "prostředí = produkční", pomocí `--metadata` parametr:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-141">The following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using the `--metadata` parameter :</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

<span data-ttu-id="4d7e9-142">Vytvoření prázdná sada záznamů záznamy lze přidat pomocí `azure network dns record-set <record-type> set-record` jak je popsáno v [vytvořit záznam DNS](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-142">Having created an empty record set, records can be added using `azure network dns record-set <record-type> set-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="4d7e9-143">Vytvořte záznamy jiné typy</span><span class="sxs-lookup"><span data-stu-id="4d7e9-143">Create records of other types</span></span>

<span data-ttu-id="4d7e9-144">S zobrazená v podrobnosti o tom, jak vytvořit záznamy 'A', následující příklady ukazují, jak vytvořit záznam jiné typy záznamů nepodporuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-144">Having seen in detail how to create 'A' records, the following examples show how to create record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="4d7e9-145">Parametry použité k zadání dat záznamu se liší podle typu záznamu.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-145">The parameters used to specify the record data vary depending on the type of the record.</span></span> <span data-ttu-id="4d7e9-146">Například pro záznam typu A zadáváte adresu IPv4 pomocí parametru `--ipv4-address <IPv4 address>`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-146">For example, for a record of type "A", you specify the IPv4 address with the parameter `--ipv4-address <IPv4 address>`.</span></span> <span data-ttu-id="4d7e9-147">Parametry pro každý typ záznamu může být uvedený pomocí `az network dns record-set <record-type> set-record --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-147">The parameters for each record type can be listed using `az network dns record-set <record-type> set-record --help`.</span></span>

<span data-ttu-id="4d7e9-148">V každém případě ukážeme, jak vytvořit jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-148">In each case, we show how to create a single record.</span></span> <span data-ttu-id="4d7e9-149">Záznam je přidat do existující sady záznamů, nebo sadu záznamů vytvořena implicitně.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-149">The record is added to the existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="4d7e9-150">Další informace o vytvoření sady záznamů a definování záznam nastavte parametr explicitně najdete v tématu [vytvoření sady záznamů DNS](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="4d7e9-151">Jsme neudělujte příklad se vytvořit sadu záznamů SOA, protože se vytvářejí SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-151">We do not give an example to create an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="4d7e9-152">Ale [SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-152">However, [the SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="4d7e9-153">Vytvoření záznamů AAAA</span><span class="sxs-lookup"><span data-stu-id="4d7e9-153">Create an AAAA record</span></span>

```azurecli
az network dns record-set aaaa set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="4d7e9-154">Vytvořte záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="4d7e9-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="4d7e9-155">Standardech DNS nepovoluje záznamy CNAME na vrcholu zóny (`--Name "@"`), ani se povolit sad záznamů obsahující více než jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-155">The DNS standards do not permit CNAME records at the apex of a zone (`--Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="4d7e9-156">Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="4d7e9-157">Vytvoření záznamů MX</span><span class="sxs-lookup"><span data-stu-id="4d7e9-157">Create an MX record</span></span>

<span data-ttu-id="4d7e9-158">V tomto příkladu vytvoříme s využitím názvu sady záznamů „@“ záznam MX ve vrcholu zóny (v tomto „contoso.com“).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-158">In this example, we use the record set name "@" to create the MX record at the zone apex (in this case, "contoso.com").</span></span>

```azurecli
az network dns record-set mx set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="4d7e9-159">Vytvoření záznamů NS</span><span class="sxs-lookup"><span data-stu-id="4d7e9-159">Create an NS record</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="4d7e9-160">Vytvořit záznam PTR</span><span class="sxs-lookup"><span data-stu-id="4d7e9-160">Create a PTR record</span></span>

<span data-ttu-id="4d7e9-161">V takovém případě se Moje-arpa-server zone.com' představuje zóny ARPA představující rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-161">In this case, 'my-arpa-zone.com' represents the ARPA zone representing your IP range.</span></span> <span data-ttu-id="4d7e9-162">Každá sada záznamů PTR v této zóně odpovídá IP adrese v rámci tohoto rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-162">Each PTR record set in this zone corresponds to an IP address within this IP range.</span></span>  <span data-ttu-id="4d7e9-163">Název záznamu "10" je poslední oktet IP adresy v tomto rozsahu IP reprezentována tento záznam.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-163">The record name '10' is the last octet of the IP address within this IP range represented by this record.</span></span>

```azurecli
az network dns record-set ptr set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a><span data-ttu-id="4d7e9-164">Vytvoření záznamů SRV</span><span class="sxs-lookup"><span data-stu-id="4d7e9-164">Create an SRV record</span></span>

<span data-ttu-id="4d7e9-165">Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte  *\_služby* a  *\_protokol* v názvu sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify the *\_service* and *\_protocol* in the record set name.</span></span> <span data-ttu-id="4d7e9-166">Není nutné zahrnout "@" v názvu sady záznamů při vytváření záznamu SRV nastavit ve vrcholu zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-166">There is no need to include "@" in the record set name when creating an SRV record set at the zone apex.</span></span>

```azurecli
az network dns record-set srv set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a><span data-ttu-id="4d7e9-167">Vytvořit záznam TXT</span><span class="sxs-lookup"><span data-stu-id="4d7e9-167">Create a TXT record</span></span>

<span data-ttu-id="4d7e9-168">Následující příklad ukazuje, jak vytvořit záznam TXT.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-168">The following example shows how to create a TXT record.</span></span> <span data-ttu-id="4d7e9-169">Další informace o maximální délku řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-169">For more information about the maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
az network dns record-set txt set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="4d7e9-170">Získání sady záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-170">Get a record set</span></span>

<span data-ttu-id="4d7e9-171">Chcete-li načíst existující sady záznamů, použijte `az network dns record-set <record-type> show`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-171">To retrieve an existing record set, use `az network dns record-set <record-type> show`.</span></span> <span data-ttu-id="4d7e9-172">Nápovědu získáte příkazem `az network dns record-set <record-type> show --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-172">For help, see `az network dns record-set <record-type> show --help`.</span></span>

<span data-ttu-id="4d7e9-173">Jako při vytváření záznamu nebo sady záznamů, musí být zadaný název sady záznamů *relativní* název, což znamená, je nutné vyloučit název zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-173">As when creating a record or record set, the record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span> <span data-ttu-id="4d7e9-174">Také budete muset zadat typ záznamu, zónu, která obsahuje sady záznamů a skupinu prostředků obsahující zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-174">You also need to specify the record type, the zone containing the record set, and the resource group containing the zone.</span></span>

<span data-ttu-id="4d7e9-175">Následující příklad načte záznam *www* a typ ze zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-175">The following example retrieves the record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a><span data-ttu-id="4d7e9-176">Seznam sad záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-176">List record sets</span></span>

<span data-ttu-id="4d7e9-177">Můžete vytvořit seznam všech záznamů v zóně DNS pomocí `az network dns record-set list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-177">You can list all records in a DNS zone by using the `az network dns record-set list` command.</span></span> <span data-ttu-id="4d7e9-178">Nápovědu získáte příkazem `az network dns record-set list --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-178">For help, see `az network dns record-set list --help`.</span></span>

<span data-ttu-id="4d7e9-179">Tento příklad vrátí sady všech záznamů v zóně *contoso.com*, ve skupině prostředků *MyResourceGroup*bez ohledu na název a typ záznamu:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-179">This example returns all record sets in the zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

<span data-ttu-id="4d7e9-180">Tento příklad vrátí všechny sady záznamů, které odpovídají daného typu záznamu (v tomto případě "A" záznamů):</span><span class="sxs-lookup"><span data-stu-id="4d7e9-180">This example returns all record sets that match the given record type (in this case, 'A' records):</span></span>

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-to-an-existing-record-set"></a><span data-ttu-id="4d7e9-181">Přidání záznamu do existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-181">Add a record to an existing record set</span></span>

<span data-ttu-id="4d7e9-182">Můžete použít `az network dns record-set <record-type> set-record` i k vytvoření záznamu v sadě záznamů nové nebo přidání záznamu do existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-182">You can use `az network dns record-set <record-type> set-record` both to create a record in a new record set, or to add a record to an existing record set.</span></span>

<span data-ttu-id="4d7e9-183">Další informace najdete v tématu [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="4d7e9-184">Odeberte záznam z existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="4d7e9-185">Chcete-li odebrat záznam DNS z existující sady záznamů, použijte `az network dns record-set <record-type> remove-record`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-185">To remove a DNS record from an existing record set, use `az network dns record-set <record-type> remove-record`.</span></span> <span data-ttu-id="4d7e9-186">Nápovědu získáte příkazem `az network dns record-set <record-type> remove-record -h`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-186">For help, see `az network dns record-set <record-type> remove-record -h`.</span></span>

<span data-ttu-id="4d7e9-187">Tento příkaz odstraní záznam DNS ze sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="4d7e9-188">Pokud se odstraní poslední záznam v sadě záznamů, je taky odstranit samotné sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-188">If the last record in a record set is deleted, the record set itself is also deleted.</span></span> <span data-ttu-id="4d7e9-189">Chcete-li zachovat prázdná místo sady záznamů, použijte `--keep-empty-record-set` možnost.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-189">To keep the empty record set instead, use the `--keep-empty-record-set` option.</span></span>

<span data-ttu-id="4d7e9-190">Je třeba zadat odstranit záznam a zóna je nutné ji odstranit, pomocí stejné parametrů jako při vytváření záznamu pomocí `az network dns record-set <record-type> set-record`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-190">You need to specify the record to be deleted and the zone it should be deleted from, using the same parameters as when creating a record using `az network dns record-set <record-type> set-record`.</span></span> <span data-ttu-id="4d7e9-191">Tyto parametry jsou popsané v [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-191">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="4d7e9-192">Následující příklad odstraní záznam A s hodnotou '1.2.3.4"ze záznamu nastavení s názvem *www* v zóně *contoso.com*, ve skupině prostředků *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-192">The following example deletes the A record with value '1.2.3.4' from the record set named *www* in the zone *contoso.com*, in the resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="4d7e9-193">Upravit existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-193">Modify an existing record set</span></span>

<span data-ttu-id="4d7e9-194">Každá sada záznamů obsahuje [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata)a záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-194">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="4d7e9-195">Následující části popisují postup úpravy každé z těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-195">The following sections explain how to modify each of these properties.</span></span>

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="4d7e9-196">Úprava záznamu A, AAAA, MX, NS, PTR, SRV a TXT</span><span class="sxs-lookup"><span data-stu-id="4d7e9-196">To modify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="4d7e9-197">Pokud chcete upravit existující záznam typu A, AAAA, MX, NS, PTR, SRV nebo TXT, by měl nejprve přidejte nový záznam a pak odstraňte stávající záznam.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-197">To modify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete the existing record.</span></span> <span data-ttu-id="4d7e9-198">Podrobné pokyny o tom, jak odstranit a přidat záznamy najdete v předchozích částech tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-198">For detailed instructions on how to delete and add records, see the earlier sections of this article.</span></span>

<span data-ttu-id="4d7e9-199">Následující příklad ukazuje, jak upravit záznam "A" z IP adresy 1.2.3.4 na IP adresu 5.6.7.8:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-199">The following example shows how to modify an 'A' record, from IP address 1.2.3.4 to IP address 5.6.7.8:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="4d7e9-200">Nelze přidat, odebrat nebo upravit záznamy v automaticky vytvořený záznam NS nastavit ve vrcholu zóny (`--Name "@"`, včetně uvozovek nahoře).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-200">You cannot add, remove, or modify the records in the automatically created NS record set at the zone apex (`--Name "@"`, including quote marks).</span></span> <span data-ttu-id="4d7e9-201">U této sady záznamů jsou povoleny pouze změny záznam upravit nastavit hodnotu TTL a metadata.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-201">For this record set, the only changes permitted are to modify the record set TTL and metadata.</span></span>

### <a name="to-modify-a-cname-record"></a><span data-ttu-id="4d7e9-202">Chcete-li upravit záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="4d7e9-202">To modify a CNAME record</span></span>

<span data-ttu-id="4d7e9-203">Na rozdíl od většině ostatních typů záznamů sadu záznamů CNAME obsahovat jenom jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-203">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="4d7e9-204">Proto nelze nahradit aktuální hodnota přidávání nového záznamu a odebráním stávajícího záznamu, jako u jiných typů záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-204">Therefore, you cannot replace the current value by adding a new record and removing the existing record, as for other record types.</span></span>

<span data-ttu-id="4d7e9-205">Místo toho pokud chcete upravit záznam CNAME, použijte `az network dns record-set cname set-record`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-205">Instead, to modify a CNAME record, use `az network dns record-set cname set-record`.</span></span> <span data-ttu-id="4d7e9-206">Nápovědu najdete v tématu`az network dns record-set cname set-record --help`</span><span class="sxs-lookup"><span data-stu-id="4d7e9-206">For help, see `az network dns record-set cname set-record --help`</span></span>

<span data-ttu-id="4d7e9-207">V příkladu upraví sady záznamů CNAME *www* v zóně *contoso.com*, ve skupině prostředků *MyResourceGroup*, přejděte na položku "www.fabrikam.net" namísto jeho existující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-207">The example modifies the CNAME record set *www* in the zone *contoso.com*, in resource group *MyResourceGroup*, to point to 'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a><span data-ttu-id="4d7e9-208">Úprava záznamu SOA</span><span class="sxs-lookup"><span data-stu-id="4d7e9-208">To modify an SOA record</span></span>

<span data-ttu-id="4d7e9-209">Na rozdíl od většině ostatních typů záznamů sadu záznamů CNAME obsahovat jenom jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-209">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="4d7e9-210">Proto nelze nahradit aktuální hodnota přidávání nového záznamu a odebráním stávajícího záznamu, jako u jiných typů záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-210">Therefore, you cannot replace the current value by adding a new record and removing the existing record, as for other record types.</span></span>

<span data-ttu-id="4d7e9-211">Místo toho použijte ke změně záznamu SOA, `az network dns record-set soa update`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-211">Instead, to modify the SOA record, use `az network dns record-set soa update`.</span></span> <span data-ttu-id="4d7e9-212">Nápovědu získáte příkazem `az network dns record-set soa update --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-212">For help, see `az network dns record-set soa update --help`.</span></span>

<span data-ttu-id="4d7e9-213">Následující příklad ukazuje, jak nastavit vlastnost 'e-mailu, záznamu SOA zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-213">The following example shows how to set the 'email' property of the SOA record for the zone *contoso.com* in the resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="4d7e9-214">Chcete-li upravit NS záznamů ve vrcholu zóny</span><span class="sxs-lookup"><span data-stu-id="4d7e9-214">To modify NS records at the zone apex</span></span>

<span data-ttu-id="4d7e9-215">S každou zónou DNS se automaticky vytvoří záznam NS nastavit ve vrcholu zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-215">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="4d7e9-216">Obsahuje názvy názvových serverů Azure DNS přiřadit do zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-216">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="4d7e9-217">Můžete přidat další název nastavit servery na tento záznam NS, podporuje společné hosting domén s více než jednoho poskytovatele DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-217">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="4d7e9-218">Můžete také upravit TTL a metadat pro tuto sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-218">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="4d7e9-219">Však nelze odebrat ani změnit předem vyplněná názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-219">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="4d7e9-220">Všimněte si, že vztahuje se pouze na vrcholu zóny sady záznamů NS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-220">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="4d7e9-221">Jiné sady záznamů NS v pásmu (jak je používá delegovat podřízených zónách) můžete změnit bez omezení.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-221">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="4d7e9-222">Následující příklad ukazuje, jak přidat do sady ve vrcholu zóny záznamů NS k další název serveru:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-222">The following example shows how to add an additional name server to the NS record set at the zone apex:</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a><span data-ttu-id="4d7e9-223">Chcete-li změnit hodnotu TTL existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-223">To modify the TTL of an existing record set</span></span>

<span data-ttu-id="4d7e9-224">Chcete-li změnit hodnotu TTL existující sady záznamů, použijte `azure network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-224">To modify the TTL of an existing record set, use `azure network dns record-set <record-type> update`.</span></span> <span data-ttu-id="4d7e9-225">Nápovědu získáte příkazem `azure network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-225">For help, see `azure network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="4d7e9-226">Následující příklad ukazuje, jak upravit sadu záznamů TTL, v takovém případě na 60 sekund:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-226">The following example shows how to modify a record set TTL, in this case to 60 seconds:</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a><span data-ttu-id="4d7e9-227">Chcete-li upravit metadata existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-227">To modify the metadata of an existing record set</span></span>

<span data-ttu-id="4d7e9-228">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze přidružit každá sada záznamů specifická data jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="4d7e9-229">Chcete-li upravit metadata existující sady záznamů, použijte `az network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-229">To modify the metadata of an existing record set, use `az network dns record-set <record-type> update`.</span></span> <span data-ttu-id="4d7e9-230">Nápovědu získáte příkazem `az network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-230">For help, see `az network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="4d7e9-231">Následující příklad ukazuje, jak upravit záznamů s dvě položky metadat "oddělení = finance" a "prostředí = produkční".</span><span class="sxs-lookup"><span data-stu-id="4d7e9-231">The following example shows how to modify a record set with two metadata entries, "dept=finance" and "environment=production".</span></span> <span data-ttu-id="4d7e9-232">Všimněte si, že všechny existující metadata jsou *nahradit* podle zadané hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-232">Note that any existing metadata is *replaced* by the values given.</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a><span data-ttu-id="4d7e9-233">Odstranit sadu záznamů</span><span class="sxs-lookup"><span data-stu-id="4d7e9-233">Delete a record set</span></span>

<span data-ttu-id="4d7e9-234">Sady záznamů lze odstranit pomocí `az network dns record-set <record-type> delete` příkaz.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-234">Record sets can be deleted by using the `az network dns record-set <record-type> delete` command.</span></span> <span data-ttu-id="4d7e9-235">Nápovědu získáte příkazem `azure network dns record-set <record-type> delete --help`.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-235">For help, see `azure network dns record-set <record-type> delete --help`.</span></span> <span data-ttu-id="4d7e9-236">Odstranění sady záznamů se také odstraní všechny záznamy v sadě záznamů.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-236">Deleting a record set also deletes all records within the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="4d7e9-237">Nelze odstranit, SOA a sady záznamů NS ve vrcholu zóny (`--name "@"`).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-237">You cannot delete the SOA and NS record sets at the zone apex (`--name "@"`).</span></span>  <span data-ttu-id="4d7e9-238">Tyto soubory jsou vytvořeny automaticky při zóny byl vytvořen a jsou automaticky odstraněna při odstranění zóny.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-238">These are created automatically when the zone was created, and are deleted automatically when the zone is deleted.</span></span>

<span data-ttu-id="4d7e9-239">Následující příklad odstraní záznam nastavení s názvem *www* a typ ze zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="4d7e9-239">The following example deletes the record set named *www* of type A from the zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

<span data-ttu-id="4d7e9-240">Zobrazí se výzva k potvrzení operace odstranění.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-240">You are prompted to confirm the delete operation.</span></span> <span data-ttu-id="4d7e9-241">Chcete-li potlačit tuto výzvu, použijte `--yes` přepínače.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-241">To suppress this prompt, use the `--yes` switch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d7e9-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4d7e9-242">Next steps</span></span>

<span data-ttu-id="4d7e9-243">Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="4d7e9-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="4d7e9-244">Zjistěte, jak [chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="4d7e9-244">Learn how to [protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
