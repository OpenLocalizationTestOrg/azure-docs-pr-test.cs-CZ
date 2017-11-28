---
title: "záznamy DNS aaaManage v Azure DNS pomocí hello 2.0 rozhraní příkazového řádku Azure | Microsoft Docs"
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
ms.openlocfilehash: 9d6f8e74ebad55ccd2381fd84a830d2c7bbb1f30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-hello-azure-cli-20"></a><span data-ttu-id="9f6df-104">Správa záznamů DNS a sady záznamů v Azure DNS pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="9f6df-104">Manage DNS records and recordsets in Azure DNS using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9f6df-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9f6df-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="9f6df-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9f6df-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="9f6df-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9f6df-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="9f6df-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9f6df-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="9f6df-109">Tento článek ukazuje, jak hello toomanage záznamy DNS pro zónu DNS pomocí rozhraní příkazového řádku Azure (CLI) 2.0, která je k dispozici pro Windows, Mac a Linux pro různé platformy.</span><span class="sxs-lookup"><span data-stu-id="9f6df-109">This article shows you how toomanage DNS records for your DNS zone by using hello cross-platform Azure command-line interface (CLI) 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="9f6df-110">Můžete také spravovat svoje záznamy DNS pomocí [prostředí Azure PowerShell](dns-operations-recordsets.md) nebo hello [portál Azure](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9f6df-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="9f6df-111">Úloha hello toocomplete verze rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="9f6df-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="9f6df-112">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="9f6df-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="9f6df-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.</span><span class="sxs-lookup"><span data-stu-id="9f6df-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="9f6df-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="9f6df-115">Hello příklady v tomto článku předpokládá, že již máte [nainstalován hello 2.0 rozhraní příkazového řádku Azure, přihlášení a vytvořit zónu DNS](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="9f6df-115">hello examples in this article assume you have already [installed hello Azure CLI 2.0, signed in, and created a DNS zone](dns-operations-dnszones-cli.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="9f6df-116">Úvod</span><span class="sxs-lookup"><span data-stu-id="9f6df-116">Introduction</span></span>

<span data-ttu-id="9f6df-117">Před vytvořením záznamy DNS v Azure DNS, musíte nejdřív toounderstand jak Azure DNS jsou uspořádány záznamy DNS do sady záznamů DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-117">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="9f6df-118">Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="9f6df-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="9f6df-119">Vytvoření záznamu DNS</span><span class="sxs-lookup"><span data-stu-id="9f6df-119">Create a DNS record</span></span>

<span data-ttu-id="9f6df-120">toocreate záznam DNS použijte hello `az network dns record-set <record-type> set-record` příkaz (kde `<record-type>` je hello typ záznamu, jednofaktorovému</span><span class="sxs-lookup"><span data-stu-id="9f6df-120">toocreate a DNS record, use hello `az network dns record-set <record-type> set-record` command (where `<record-type>` is hello type of record, i.e</span></span> <span data-ttu-id="9f6df-121">a, srv, txt, atd.) Nápovědu získáte příkazem `az network dns record-set --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-121">a, srv, txt, etc.) For help, see `az network dns record-set --help`.</span></span>

<span data-ttu-id="9f6df-122">Při vytváření záznamu, je třeba název skupiny prostředků hello toospecify, název zóny, název sady záznamů, typ záznamu hello a podrobnosti hello hello záznamu vytváří.</span><span class="sxs-lookup"><span data-stu-id="9f6df-122">When creating a record, you need toospecify hello resource group name, zone name, record set name, hello record type, and hello details of hello record being created.</span></span> <span data-ttu-id="9f6df-123">Hello název sady záznamů musí být *relativní* název, což znamená, je nutné vyloučit hello název zóny.</span><span class="sxs-lookup"><span data-stu-id="9f6df-123">hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span>

<span data-ttu-id="9f6df-124">Pokud sada záznamů hello již neexistuje, tento příkaz se automaticky vytvoří.</span><span class="sxs-lookup"><span data-stu-id="9f6df-124">If hello record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="9f6df-125">Pokud již sadu záznamů hello existuje, tento příkaz přidá záznam hello zadáte toohello existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f6df-125">If hello record set already exists, this command adds hello record you specify toohello existing record set.</span></span>

<span data-ttu-id="9f6df-126">Pokud se vytváří nová sada záznamů, použije se výchozí hodnota TTL (Time to Live) 3 600.</span><span class="sxs-lookup"><span data-stu-id="9f6df-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="9f6df-127">Pokyny, jak toouse různých TTLs, najdete v části [vytvoření sady záznamů DNS](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="9f6df-127">For instructions on how toouse different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="9f6df-128">Hello následující příklad vytvoří záznam názvem *www* v zóně hello *contoso.com* ve skupině prostředků hello *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="9f6df-128">hello following example creates an A record called *www* in hello zone *contoso.com* in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="9f6df-129">Hello IP adresu hello je záznam *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="9f6df-129">hello IP address of hello A record is *1.2.3.4*.</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="9f6df-130">nastavit toocreate záznam v hello vrcholu zóny hello (v tomto případě "contoso.com"), použijte hello název záznamu "@", včetně uvozovek hello:</span><span class="sxs-lookup"><span data-stu-id="9f6df-130">toocreate a record set in hello apex of hello zone (in this case, "contoso.com"), use hello record name "@", including hello quotation marks:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="9f6df-131">Vytvoření sady záznamů DNS</span><span class="sxs-lookup"><span data-stu-id="9f6df-131">Create a DNS record set</span></span>

<span data-ttu-id="9f6df-132">V hello výše příklady záznam DNS hello byla buď přidané tooan existující sady záznamů, nebo bylo vytvořeno sady záznamů hello *implicitně*.</span><span class="sxs-lookup"><span data-stu-id="9f6df-132">In hello above examples, hello DNS record was either added tooan existing record set, or hello record set was created *implicitly*.</span></span> <span data-ttu-id="9f6df-133">Můžete také vytvořit sadu záznamů hello *explicitně* před přidávání záznamů tooit.</span><span class="sxs-lookup"><span data-stu-id="9f6df-133">You can also create hello record set *explicitly* before adding records tooit.</span></span> <span data-ttu-id="9f6df-134">Azure DNS podporuje 'prázdné, sady záznamů, které může fungovat jako zástupný symbol tooreserve název DNS před vytvořením záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-134">Azure DNS supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="9f6df-135">Prázdný sady záznamů jsou viditelné v rovině řízení Azure DNS, ale nejsou zobrazeny na názvových serverů Azure DNS hello hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-135">Empty record sets are visible in hello Azure DNS control plane, but do not appear on hello Azure DNS name servers.</span></span>

<span data-ttu-id="9f6df-136">Sady záznamů jsou vytvořené pomocí hello `az network dns record-set <record-type> create` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9f6df-136">Record sets are created using hello `az network dns record-set <record-type> create` command.</span></span> <span data-ttu-id="9f6df-137">Nápovědu získáte příkazem `az network dns record-set <record-type> create --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-137">For help, see `az network dns record-set <record-type> create --help`.</span></span>

<span data-ttu-id="9f6df-138">Vytvoření záznamu hello explicitně nastaven umožňuje vlastnosti sady záznamů toospecify například hello [Time To Live (TTL)](dns-zones-records.md#time-to-live) a metadata.</span><span class="sxs-lookup"><span data-stu-id="9f6df-138">Creating hello record set explicitly allows you toospecify record set properties such as hello [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="9f6df-139">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="9f6df-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="9f6df-140">Hello následující příklad vytvoří prázdná sada záznamů typu "A" s hodnotou TTL 60 sekund pomocí hello `--ttl` parametr (krátký tvar `-l`):</span><span class="sxs-lookup"><span data-stu-id="9f6df-140">hello following example creates an empty record set of type 'A' with a 60-second TTL, by using hello `--ttl` parameter (short form `-l`):</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

<span data-ttu-id="9f6df-141">Hello následující příklad vytvoří záznam s dvě položky metadat "oddělení = finance" a "prostředí = produkční", pomocí hello `--metadata` parametr:</span><span class="sxs-lookup"><span data-stu-id="9f6df-141">hello following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using hello `--metadata` parameter :</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

<span data-ttu-id="9f6df-142">Vytvoření prázdná sada záznamů záznamy lze přidat pomocí `azure network dns record-set <record-type> set-record` jak je popsáno v [vytvořit záznam DNS](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="9f6df-142">Having created an empty record set, records can be added using `azure network dns record-set <record-type> set-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="9f6df-143">Vytvořte záznamy jiné typy</span><span class="sxs-lookup"><span data-stu-id="9f6df-143">Create records of other types</span></span>

<span data-ttu-id="9f6df-144">S vidět podrobně jak toocreate "A" záznamy, hello následující příklady zobrazují jak toocreate záznam jiné typy záznamů podporované Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-144">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="9f6df-145">Parametry Hello použít toospecify hello záznam, který dat se liší v závislosti na typu hello hello záznamu.</span><span class="sxs-lookup"><span data-stu-id="9f6df-145">hello parameters used toospecify hello record data vary depending on hello type of hello record.</span></span> <span data-ttu-id="9f6df-146">Například pro záznam typu "A", jste zadali hello IPv4 adresu s parametrem hello `--ipv4-address <IPv4 address>`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-146">For example, for a record of type "A", you specify hello IPv4 address with hello parameter `--ipv4-address <IPv4 address>`.</span></span> <span data-ttu-id="9f6df-147">Hello parametry pro každý typ záznamu může být uvedený pomocí `az network dns record-set <record-type> set-record --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-147">hello parameters for each record type can be listed using `az network dns record-set <record-type> set-record --help`.</span></span>

<span data-ttu-id="9f6df-148">V každém případě ukážeme, jak toocreate jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="9f6df-148">In each case, we show how toocreate a single record.</span></span> <span data-ttu-id="9f6df-149">záznam Hello je přidaný toohello existující sady záznamů nebo vytvořit sadu záznamů implicitně.</span><span class="sxs-lookup"><span data-stu-id="9f6df-149">hello record is added toohello existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="9f6df-150">Další informace o vytvoření sady záznamů a definování záznam nastavte parametr explicitně najdete v tématu [vytvoření sady záznamů DNS](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="9f6df-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="9f6df-151">Jsme neudělujte toocreate příklad sady záznamů SOA vzhledem k tomu, že se vytvoří SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně.</span><span class="sxs-lookup"><span data-stu-id="9f6df-151">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="9f6df-152">Ale [hello SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="9f6df-152">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="9f6df-153">Vytvoření záznamů AAAA</span><span class="sxs-lookup"><span data-stu-id="9f6df-153">Create an AAAA record</span></span>

```azurecli
az network dns record-set aaaa set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="9f6df-154">Vytvořte záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="9f6df-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="9f6df-155">standardech DNS Hello nepovoluje záznamy CNAME v hello vrcholu zóny (`--Name "@"`), ani se povolit sad záznamů obsahující více než jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="9f6df-155">hello DNS standards do not permit CNAME records at hello apex of a zone (`--Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="9f6df-156">Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="9f6df-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="9f6df-157">Vytvoření záznamů MX</span><span class="sxs-lookup"><span data-stu-id="9f6df-157">Create an MX record</span></span>

<span data-ttu-id="9f6df-158">V tomto příkladu používáme hello název sady záznamů "@" hello toocreate záznam MX ve vrcholu zóny hello (v tomto případě "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="9f6df-158">In this example, we use hello record set name "@" toocreate hello MX record at hello zone apex (in this case, "contoso.com").</span></span>

```azurecli
az network dns record-set mx set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="9f6df-159">Vytvoření záznamů NS</span><span class="sxs-lookup"><span data-stu-id="9f6df-159">Create an NS record</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="9f6df-160">Vytvořit záznam PTR</span><span class="sxs-lookup"><span data-stu-id="9f6df-160">Create a PTR record</span></span>

<span data-ttu-id="9f6df-161">V takovém případě se Moje-arpa-server zone.com' představuje hello zóny ARPA představující rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="9f6df-161">In this case, 'my-arpa-zone.com' represents hello ARPA zone representing your IP range.</span></span> <span data-ttu-id="9f6df-162">Každý záznam PTR nastavení v této zóně odpovídá tooan IP adresu do tohoto rozsahu IP adres.</span><span class="sxs-lookup"><span data-stu-id="9f6df-162">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span>  <span data-ttu-id="9f6df-163">název záznamu Hello "10" je poslední oktet hello hello IP adresy v tomto rozsahu IP reprezentována tento záznam.</span><span class="sxs-lookup"><span data-stu-id="9f6df-163">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```azurecli
az network dns record-set ptr set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a><span data-ttu-id="9f6df-164">Vytvoření záznamů SRV</span><span class="sxs-lookup"><span data-stu-id="9f6df-164">Create an SRV record</span></span>

<span data-ttu-id="9f6df-165">Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte hello  *\_služby* a  *\_protokol* v hello název sady záznamu.</span><span class="sxs-lookup"><span data-stu-id="9f6df-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="9f6df-166">Neexistuje žádné tooinclude nutné "@" v hello název sady záznamu při vytváření záznamu SRV nastavit na vrcholu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-166">There is no need tooinclude "@" in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```azurecli
az network dns record-set srv set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a><span data-ttu-id="9f6df-167">Vytvořit záznam TXT</span><span class="sxs-lookup"><span data-stu-id="9f6df-167">Create a TXT record</span></span>

<span data-ttu-id="9f6df-168">Hello následující příklad ukazuje, jak záznam toocreate TXT.</span><span class="sxs-lookup"><span data-stu-id="9f6df-168">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="9f6df-169">Další informace o hello maximální délka řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="9f6df-169">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
az network dns record-set txt set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="9f6df-170">Získání sady záznamů</span><span class="sxs-lookup"><span data-stu-id="9f6df-170">Get a record set</span></span>

<span data-ttu-id="9f6df-171">tooretrieve existující sady záznamů, použijte `az network dns record-set <record-type> show`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-171">tooretrieve an existing record set, use `az network dns record-set <record-type> show`.</span></span> <span data-ttu-id="9f6df-172">Nápovědu získáte příkazem `az network dns record-set <record-type> show --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-172">For help, see `az network dns record-set <record-type> show --help`.</span></span>

<span data-ttu-id="9f6df-173">Při vytváření záznamu nebo sady záznamů, sady záznamů hello název musí mít *relativní* název, což znamená, je nutné vyloučit hello název zóny.</span><span class="sxs-lookup"><span data-stu-id="9f6df-173">As when creating a record or record set, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="9f6df-174">Musíte taky typ záznamu hello toospecify, hello zóny hello záznam obsahující nastavení a hello skupinu prostředků obsahující hello zóny.</span><span class="sxs-lookup"><span data-stu-id="9f6df-174">You also need toospecify hello record type, hello zone containing hello record set, and hello resource group containing hello zone.</span></span>

<span data-ttu-id="9f6df-175">Hello následující příklad načte záznam hello *www* a typ ze zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="9f6df-175">hello following example retrieves hello record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a><span data-ttu-id="9f6df-176">Seznam sad záznamů</span><span class="sxs-lookup"><span data-stu-id="9f6df-176">List record sets</span></span>

<span data-ttu-id="9f6df-177">Můžete vytvořit seznam všech záznamů v zóně DNS pomocí hello `az network dns record-set list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9f6df-177">You can list all records in a DNS zone by using hello `az network dns record-set list` command.</span></span> <span data-ttu-id="9f6df-178">Nápovědu získáte příkazem `az network dns record-set list --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-178">For help, see `az network dns record-set list --help`.</span></span>

<span data-ttu-id="9f6df-179">Tento příklad vrátí sady všech záznamů v zóně hello *contoso.com*, ve skupině prostředků *MyResourceGroup*bez ohledu na název a typ záznamu:</span><span class="sxs-lookup"><span data-stu-id="9f6df-179">This example returns all record sets in hello zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

<span data-ttu-id="9f6df-180">Tento příklad vrátí všechny sady záznamů, které odpovídají hello daného typu záznamu (v tomto případě "A" záznamů):</span><span class="sxs-lookup"><span data-stu-id="9f6df-180">This example returns all record sets that match hello given record type (in this case, 'A' records):</span></span>

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="9f6df-181">Přidání záznamů tooan existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="9f6df-181">Add a record tooan existing record set</span></span>

<span data-ttu-id="9f6df-182">Můžete použít `az network dns record-set <record-type> set-record` sada záznamů v novém záznamu toocreate i tooadd stávající záznam záznamů tooan nastavit.</span><span class="sxs-lookup"><span data-stu-id="9f6df-182">You can use `az network dns record-set <record-type> set-record` both toocreate a record in a new record set, or tooadd a record tooan existing record set.</span></span>

<span data-ttu-id="9f6df-183">Další informace najdete v tématu [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.</span><span class="sxs-lookup"><span data-stu-id="9f6df-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="9f6df-184">Odeberte záznam z existující sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f6df-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="9f6df-185">tooremove DNS záznam z existující sady záznamů, použijte `az network dns record-set <record-type> remove-record`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-185">tooremove a DNS record from an existing record set, use `az network dns record-set <record-type> remove-record`.</span></span> <span data-ttu-id="9f6df-186">Nápovědu získáte příkazem `az network dns record-set <record-type> remove-record -h`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-186">For help, see `az network dns record-set <record-type> remove-record -h`.</span></span>

<span data-ttu-id="9f6df-187">Tento příkaz odstraní záznam DNS ze sady záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f6df-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="9f6df-188">Pokud se odstraní poslední záznam hello v sadě záznamů, je taky odstranit samotné sady záznamů hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-188">If hello last record in a record set is deleted, hello record set itself is also deleted.</span></span> <span data-ttu-id="9f6df-189">Místo toho použijte hello tookeep hello prázdná sada záznamů `--keep-empty-record-set` možnost.</span><span class="sxs-lookup"><span data-stu-id="9f6df-189">tookeep hello empty record set instead, use hello `--keep-empty-record-set` option.</span></span>

<span data-ttu-id="9f6df-190">Je třeba toospecify hello záznamů toobe odstranit a hello zónu, je nutné ji odstranit, pomocí hello stejnými parametry, jako při vytváření záznamů pomocí `az network dns record-set <record-type> set-record`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-190">You need toospecify hello record toobe deleted and hello zone it should be deleted from, using hello same parameters as when creating a record using `az network dns record-set <record-type> set-record`.</span></span> <span data-ttu-id="9f6df-191">Tyto parametry jsou popsané v [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.</span><span class="sxs-lookup"><span data-stu-id="9f6df-191">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="9f6df-192">Následující příklad odstranění Hello hello záznam s hodnotou '1.2.3.4"ze záznamu hello nastavení s názvem *www* v zóně hello *contoso.com*, ve skupině prostředků hello *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="9f6df-192">hello following example deletes hello A record with value '1.2.3.4' from hello record set named *www* in hello zone *contoso.com*, in hello resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="9f6df-193">Upravit existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="9f6df-193">Modify an existing record set</span></span>

<span data-ttu-id="9f6df-194">Každá sada záznamů obsahuje [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata)a záznamy DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-194">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="9f6df-195">Hello následující oddíly popisují, jak toomodify těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="9f6df-195">hello following sections explain how toomodify each of these properties.</span></span>

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="9f6df-196">toomodify záznam A, AAAA, MX, NS, PTR, SRV a TXT</span><span class="sxs-lookup"><span data-stu-id="9f6df-196">toomodify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="9f6df-197">toomodify existujícího záznamu typu A, AAAA, MX, NS, PTR, SRV nebo TXT, měli byste nejprve přidat nový záznam a pak odstraňte hello stávajícího záznamu.</span><span class="sxs-lookup"><span data-stu-id="9f6df-197">toomodify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete hello existing record.</span></span> <span data-ttu-id="9f6df-198">Pro podrobné pokyny, jak toodelete a přidat záznamy, najdete v části hello dřívější části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="9f6df-198">For detailed instructions on how toodelete and add records, see hello earlier sections of this article.</span></span>

<span data-ttu-id="9f6df-199">Hello následující příklad ukazuje, jak toomodify záznam "A", z IP adres 1.2.3.4 tooIP adres 5.6.7.8:</span><span class="sxs-lookup"><span data-stu-id="9f6df-199">hello following example shows how toomodify an 'A' record, from IP address 1.2.3.4 tooIP address 5.6.7.8:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="9f6df-200">Nelze přidat, odebrat nebo změnit hello záznamy v hello automaticky vytvořil záznam NS nastavit na vrcholu zóny hello (`--Name "@"`, včetně uvozovek nahoře).</span><span class="sxs-lookup"><span data-stu-id="9f6df-200">You cannot add, remove, or modify hello records in hello automatically created NS record set at hello zone apex (`--Name "@"`, including quote marks).</span></span> <span data-ttu-id="9f6df-201">Pro tuto sadu záznamů hello povoleny pouze změny jsou sady záznamů hello toomodify TTL a metadata.</span><span class="sxs-lookup"><span data-stu-id="9f6df-201">For this record set, hello only changes permitted are toomodify hello record set TTL and metadata.</span></span>

### <a name="toomodify-a-cname-record"></a><span data-ttu-id="9f6df-202">toomodify záznam CNAME</span><span class="sxs-lookup"><span data-stu-id="9f6df-202">toomodify a CNAME record</span></span>

<span data-ttu-id="9f6df-203">Na rozdíl od většině ostatních typů záznamů sadu záznamů CNAME obsahovat jenom jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="9f6df-203">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="9f6df-204">Proto nelze nahradit aktuální hodnota hello přidáním nového záznamu a odebrání hello stávajícího záznamu, jako u jiných typů záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f6df-204">Therefore, you cannot replace hello current value by adding a new record and removing hello existing record, as for other record types.</span></span>

<span data-ttu-id="9f6df-205">Místo toho použijte toomodify záznam CNAME `az network dns record-set cname set-record`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-205">Instead, toomodify a CNAME record, use `az network dns record-set cname set-record`.</span></span> <span data-ttu-id="9f6df-206">Nápovědu najdete v tématu`az network dns record-set cname set-record --help`</span><span class="sxs-lookup"><span data-stu-id="9f6df-206">For help, see `az network dns record-set cname set-record --help`</span></span>

<span data-ttu-id="9f6df-207">Hello příklad upravuje sadu záznamů CNAME hello *www* v zóně hello *contoso.com*, ve skupině prostředků *MyResourceGroup*, toopoint příliš 'www.fabrikam.net"namísto jeho stávající hodnotu:</span><span class="sxs-lookup"><span data-stu-id="9f6df-207">hello example modifies hello CNAME record set *www* in hello zone *contoso.com*, in resource group *MyResourceGroup*, toopoint too'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="9f6df-208">toomodify záznamu SOA</span><span class="sxs-lookup"><span data-stu-id="9f6df-208">toomodify an SOA record</span></span>

<span data-ttu-id="9f6df-209">Na rozdíl od většině ostatních typů záznamů sadu záznamů CNAME obsahovat jenom jeden záznam.</span><span class="sxs-lookup"><span data-stu-id="9f6df-209">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="9f6df-210">Proto nelze nahradit aktuální hodnota hello přidáním nového záznamu a odebrání hello stávajícího záznamu, jako u jiných typů záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f6df-210">Therefore, you cannot replace hello current value by adding a new record and removing hello existing record, as for other record types.</span></span>

<span data-ttu-id="9f6df-211">Místo toho použijte toomodify hello záznam SOA `az network dns record-set soa update`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-211">Instead, toomodify hello SOA record, use `az network dns record-set soa update`.</span></span> <span data-ttu-id="9f6df-212">Nápovědu získáte příkazem `az network dns record-set soa update --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-212">For help, see `az network dns record-set soa update --help`.</span></span>

<span data-ttu-id="9f6df-213">Hello následující příklad ukazuje, jak vlastnost e-mailu, tooset hello"hello SOA záznam pro zóny hello *contoso.com* ve skupině prostředků hello *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="9f6df-213">hello following example shows how tooset hello 'email' property of hello SOA record for hello zone *contoso.com* in hello resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="9f6df-214">toomodify NS záznamy na vrcholu zóny hello</span><span class="sxs-lookup"><span data-stu-id="9f6df-214">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="9f6df-215">Nastavte na vrcholu zóny hello záznam NS Hello se vytvoří automaticky s každou zónou DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-215">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="9f6df-216">Obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny.</span><span class="sxs-lookup"><span data-stu-id="9f6df-216">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="9f6df-217">Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény.</span><span class="sxs-lookup"><span data-stu-id="9f6df-217">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="9f6df-218">Můžete také upravit hello TTL a metadat pro tuto sadu záznamů.</span><span class="sxs-lookup"><span data-stu-id="9f6df-218">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="9f6df-219">Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-219">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="9f6df-220">Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-220">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="9f6df-221">Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) můžete změnit bez omezení.</span><span class="sxs-lookup"><span data-stu-id="9f6df-221">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="9f6df-222">Hello následující příklad ukazuje, jak tooadd záznam NS toohello další název serveru nastavit na vrcholu zóny hello:</span><span class="sxs-lookup"><span data-stu-id="9f6df-222">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a><span data-ttu-id="9f6df-223">nastavit toomodify hello TTL existujícího záznamu</span><span class="sxs-lookup"><span data-stu-id="9f6df-223">toomodify hello TTL of an existing record set</span></span>

<span data-ttu-id="9f6df-224">nastavení toomodify hello TTL existujícího záznamu, použijte `azure network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-224">toomodify hello TTL of an existing record set, use `azure network dns record-set <record-type> update`.</span></span> <span data-ttu-id="9f6df-225">Nápovědu získáte příkazem `azure network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-225">For help, see `azure network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="9f6df-226">Hello následující příklad ukazuje, jak toomodify sadu záznamů TTL, v tomto případě too60 sekund:</span><span class="sxs-lookup"><span data-stu-id="9f6df-226">hello following example shows how toomodify a record set TTL, in this case too60 seconds:</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a><span data-ttu-id="9f6df-227">metadata hello toomodify existující sady záznamů</span><span class="sxs-lookup"><span data-stu-id="9f6df-227">toomodify hello metadata of an existing record set</span></span>

<span data-ttu-id="9f6df-228">[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="9f6df-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="9f6df-229">nastavení toomodify hello metadata existujícího záznamu, použijte `az network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-229">toomodify hello metadata of an existing record set, use `az network dns record-set <record-type> update`.</span></span> <span data-ttu-id="9f6df-230">Nápovědu získáte příkazem `az network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-230">For help, see `az network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="9f6df-231">Hello následující příklad ukazuje, jak toomodify sada záznamů s dvě položky metadat "oddělení = finance" a "prostředí = produkční".</span><span class="sxs-lookup"><span data-stu-id="9f6df-231">hello following example shows how toomodify a record set with two metadata entries, "dept=finance" and "environment=production".</span></span> <span data-ttu-id="9f6df-232">Všimněte si, že všechny existující metadata jsou *nahradit* podle zadané hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-232">Note that any existing metadata is *replaced* by hello values given.</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a><span data-ttu-id="9f6df-233">Odstranit sadu záznamů</span><span class="sxs-lookup"><span data-stu-id="9f6df-233">Delete a record set</span></span>

<span data-ttu-id="9f6df-234">Sady záznamů lze odstranit pomocí hello `az network dns record-set <record-type> delete` příkaz.</span><span class="sxs-lookup"><span data-stu-id="9f6df-234">Record sets can be deleted by using hello `az network dns record-set <record-type> delete` command.</span></span> <span data-ttu-id="9f6df-235">Nápovědu získáte příkazem `azure network dns record-set <record-type> delete --help`.</span><span class="sxs-lookup"><span data-stu-id="9f6df-235">For help, see `azure network dns record-set <record-type> delete --help`.</span></span> <span data-ttu-id="9f6df-236">Odstranění sady záznamů také odstraní všechny záznamy v sadě záznamů hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-236">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="9f6df-237">Nelze odstranit hello SOA a NS sady záznamů na vrcholu zóny hello (`--name "@"`).</span><span class="sxs-lookup"><span data-stu-id="9f6df-237">You cannot delete hello SOA and NS record sets at hello zone apex (`--name "@"`).</span></span>  <span data-ttu-id="9f6df-238">Tyto soubory jsou vytvořeny automaticky při hello zóny byl vytvořen a jsou automaticky odstraněna při odstranění zóny hello.</span><span class="sxs-lookup"><span data-stu-id="9f6df-238">These are created automatically when hello zone was created, and are deleted automatically when hello zone is deleted.</span></span>

<span data-ttu-id="9f6df-239">Hello následující příklad odstraní sadu s názvem záznamů hello *www* a typ ze zóny hello *contoso.com* ve skupině prostředků *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="9f6df-239">hello following example deletes hello record set named *www* of type A from hello zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

<span data-ttu-id="9f6df-240">Operace odstranění hello výzvami tooconfirm jste.</span><span class="sxs-lookup"><span data-stu-id="9f6df-240">You are prompted tooconfirm hello delete operation.</span></span> <span data-ttu-id="9f6df-241">toosuppress tento řádek, použijte hello `--yes` přepínače.</span><span class="sxs-lookup"><span data-stu-id="9f6df-241">toosuppress this prompt, use hello `--yes` switch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f6df-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9f6df-242">Next steps</span></span>

<span data-ttu-id="9f6df-243">Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="9f6df-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="9f6df-244">Zjistěte, jak příliš[chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9f6df-244">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
