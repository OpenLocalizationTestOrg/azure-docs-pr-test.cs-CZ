---
title: "aaaHosting DNS zóny zpětného vyhledávání v Azure DNS | Microsoft Docs"
description: "Zjistěte, jak toouse Azure DNS toohost hello DNS zóny zpětného vyhledávání pro váš rozsahy IP adres"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 24feb8ef1c75a7d91938867f348fed1190046e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a><span data-ttu-id="9321d-103">Hostování zpětné zóny DNS vyhledávání v Azure DNS</span><span class="sxs-lookup"><span data-stu-id="9321d-103">Hosting reverse DNS lookup zones in Azure DNS</span></span>

<span data-ttu-id="9321d-104">Tento článek vysvětluje, jak toohost hello DNS zóny zpětného vyhledávání pro váš přiřazené rozsahy IP v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-104">This article explains how toohost hello reverse DNS lookup zones for your assigned IP ranges in Azure DNS.</span></span> <span data-ttu-id="9321d-105">rozsahy IP Hello reprezentována zóny zpětného vyhledávání hello musí být přiřazen tooyour organizace, obvykle podle vašeho poskytovatele internetových služeb.</span><span class="sxs-lookup"><span data-stu-id="9321d-105">hello IP ranges represented by hello reverse lookup zone must be assigned tooyour organization, typically by your ISP.</span></span>

<span data-ttu-id="9321d-106">tooconfigure zpětně DNS pro Azure vlastní IP adresu přiřazenou tooyour služba Azure, najdete v části [konfigurovat hello zpětného vyhledávání pro hello IP adresy přidělené tooyour služba Azure](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-106">tooconfigure reverse DNS for Azure-owned IP address assigned tooyour Azure service, see [configure hello reverse lookup for hello IP addresses allocated tooyour Azure service](dns-reverse-dns-for-azure-services.md).</span></span>

<span data-ttu-id="9321d-107">Před přečtení tohoto článku, měli byste se seznámit s tím [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-107">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="9321d-108">Tento článek vás provede kroky toocreate hello první zóny zpětného vyhledávání DNS a záznam pomocí hello portál Azure, Azure PowerShell, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="9321d-108">This article walks you through hello steps toocreate your first reverse lookup DNS zone and record using hello Azure portal, Azure PowerShell, Azure CLI 1.0 or Azure CLI 2.0.</span></span>

## <a name="create-a-reverse-lookup-dns-zone"></a><span data-ttu-id="9321d-109">Vytvořit zónu zpětného vyhledávání DNS</span><span class="sxs-lookup"><span data-stu-id="9321d-109">Create a reverse lookup DNS zone</span></span>

1. <span data-ttu-id="9321d-110">Přihlaste se toohello [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="9321d-110">Sign in toohello [Azure portal](https://portal.azure.com)</span></span>
1. <span data-ttu-id="9321d-111">V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový** > **sítě** > a pak klikněte na **zónu DNS** tooopen hello **zóny DNS vytvořte**okno.</span><span class="sxs-lookup"><span data-stu-id="9321d-111">On hello Hub menu, click and click **New** > **Networking** > and then click **DNS zone** tooopen hello **Create DNS zone** blade.</span></span>

   ![Zóna DNS](./media/dns-reverse-dns-hosting/figure1.png)

1. <span data-ttu-id="9321d-113">Na hello **zóny DNS vytvořte** okně název zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-113">On hello **Create DNS zone** blade, name your DNS zone.</span></span> <span data-ttu-id="9321d-114">Hello název zóny hello je jinak vytvořených pro předpony IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="9321d-114">hello name of hello zone is crafted differently for IPv4 and IPv6 prefixes.</span></span> <span data-ttu-id="9321d-115">Použijte buď hello pokyny pro [IPV4](#ipv4) nebo [IPv6](#ipv6) tooname vaši zónu.</span><span class="sxs-lookup"><span data-stu-id="9321d-115">Use either hello instructions for [IPV4](#ipv4) or [IPv6](#ipv6) tooname your zone.</span></span> <span data-ttu-id="9321d-116">Po dokončení klikněte na tlačítko **vytvořit** toocreate hello zóny.</span><span class="sxs-lookup"><span data-stu-id="9321d-116">When complete click **Create** toocreate hello zone.</span></span>

### <a name="ipv4"></a><span data-ttu-id="9321d-117">IPv4</span><span class="sxs-lookup"><span data-stu-id="9321d-117">IPv4</span></span>

<span data-ttu-id="9321d-118">Název Hello zóny zpětného vyhledávání IPv4 je založen na rozsah hello IP adres, který představuje.</span><span class="sxs-lookup"><span data-stu-id="9321d-118">hello name of an IPv4 reverse lookup zone is based on hello IP range it represents.</span></span> <span data-ttu-id="9321d-119">Musí být ve formátu hello: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="9321d-119">It should be in hello following format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span></span> <span data-ttu-id="9321d-120">Příklady najdete v tématu [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md#ipv4).</span><span class="sxs-lookup"><span data-stu-id="9321d-120">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv4).</span></span>

> [!NOTE]
> <span data-ttu-id="9321d-121">Při vytváření classless zóny zpětného vyhledávání DNS v Azure DNS, je nutné použít pomlčka (`-`) namísto lomítkem (/') v názvu zóny hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-121">When creating classless reverse DNS lookup zones in Azure DNS, you must use a hyphen (`-`) rather than a forward slash ('/') in hello zone name.</span></span>
>
> <span data-ttu-id="9321d-122">Například pro hello 192.0.2.128/26 rozsah IP, musíte použít `128-26.2.0.192.in-addr.arpa` jako název zóny hello místo `128/26.2.0.192.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="9321d-122">For example, for hello IP range 192.0.2.128/26, you must use `128-26.2.0.192.in-addr.arpa` as hello zone name instead of `128/26.2.0.192.in-addr.arpa`.</span></span>
>
> <span data-ttu-id="9321d-123">Důvodem je, že když jsou oba podporované ve standardech DNS hello, zón služby DNS názvy obsahující hello lomítkem (`/`) znak nejsou podporovány v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-123">This is because, while both are supported by hello DNS standards, DNS zone names containing hello forward slash (`/`) character are not supported in Azure DNS.</span></span>

<span data-ttu-id="9321d-124">Hello následující příklad ukazuje, jak toocreate třídy C reverse zónu DNS s názvem `2.0.192.in-addr.arpa` v Azure DNS prostřednictvím hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="9321d-124">hello following example shows how toocreate a 'Class C' reverse DNS zone named `2.0.192.in-addr.arpa` in Azure DNS via hello Azure portal:</span></span>

 ![Vytvoření zóny DNS](./media/dns-reverse-dns-hosting/figure2.png)

<span data-ttu-id="9321d-126">Definuje hello umístění pro skupinu prostředků hello Hello umístění skupiny prostředků a nemá žádný vliv na zónu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-126">hello 'Resource group location' defines hello location for hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="9321d-127">umístění zóny DNS Hello je vždy "globální a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="9321d-127">hello DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="9321d-128">Hello následující příklady ukazují, jak toocomplete tato úloha pomocí Azure PowerShell a rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="9321d-128">hello following examples show how toocomplete this task with Azure PowerShell and hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="9321d-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9321d-129">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="9321d-130">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9321d-130">Azure CLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="9321d-131">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9321d-131">Azure CLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="9321d-132">IPv6</span><span class="sxs-lookup"><span data-stu-id="9321d-132">IPv6</span></span>

<span data-ttu-id="9321d-133">Název Hello zóny zpětného vyhledávání IPv6 by měl být v hello následující formulář: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span><span class="sxs-lookup"><span data-stu-id="9321d-133">hello name of an IPv6 reverse lookup zone should be in hello following form: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span></span>  <span data-ttu-id="9321d-134">Příklady najdete v tématu [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md#ipv6).</span><span class="sxs-lookup"><span data-stu-id="9321d-134">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv6).</span></span>


<span data-ttu-id="9321d-135">Hello následující příklad ukazuje, jak toocreate zóny vyhledávání DNS zpětného IPv6 s názvem `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` v Azure DNS prostřednictvím hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="9321d-135">hello following example shows how toocreate an IPv6 reverse DNS lookup zone named `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via hello Azure portal:</span></span>

 ![Vytvoření zóny DNS](./media/dns-reverse-dns-hosting/figure3.png)

<span data-ttu-id="9321d-137">Definuje hello umístění pro skupinu prostředků hello Hello umístění skupiny prostředků a nemá žádný vliv na zónu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-137">hello 'Resource group location' defines hello location for hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="9321d-138">umístění zóny DNS Hello je vždy "globální a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="9321d-138">hello DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="9321d-139">Hello následující příklady ukazují, jak toocomplete tato úloha pomocí Azure PowerShell a rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="9321d-139">hello following examples show how toocomplete this task with Azure PowerShell and hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="9321d-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9321d-140">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a><span data-ttu-id="9321d-141">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9321d-141">AzureCLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a><span data-ttu-id="9321d-142">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9321d-142">AzureCLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a><span data-ttu-id="9321d-143">Delegování zóny zpětného vyhledávání DNS</span><span class="sxs-lookup"><span data-stu-id="9321d-143">Delegate a reverse DNS lookup zone</span></span>

<span data-ttu-id="9321d-144">Vytvoření zóny zpětného vyhledávání DNS je nutné zajistit, že je zóny hello delegovaná z nadřazené zóně hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-144">Having created your reverse DNS lookup zone, you must ensure that hello zone is delegated from hello parent zone.</span></span> <span data-ttu-id="9321d-145">Delegování DNS umožňuje hello DNS název řešení proces toofind hello názvové servery hostující vaše zóny zpětného vyhledávání DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-145">DNS delegation enables hello DNS name resolution process toofind hello name servers hosting your reverse DNS lookup zone.</span></span> <span data-ttu-id="9321d-146">To umožňuje tyto název servery tooanswer zpětné dotazy DNS pro hello IP adresy ve vaší rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="9321d-146">This enables those name servers tooanswer DNS reverse queries for hello IP addresses in your address range.</span></span>

<span data-ttu-id="9321d-147">Pro zóny dopředného vyhledávání, je popsán proces hello delegování zóny DNS v [delegovat tooAzure vaší domény DNS](dns-delegate-domain-azure-dns.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-147">For forward lookup zones, hello process of delegating a DNS zone is described in [Delegate your domain tooAzure DNS](dns-delegate-domain-azure-dns.md).</span></span> <span data-ttu-id="9321d-148">Delegování zón zpětného vyhledávání funguje hello stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="9321d-148">Delegation for reverse lookup zones works hello same way.</span></span> <span data-ttu-id="9321d-149">Hello jediným rozdílem je, že je potřeba tooconfigure hello názvové servery s hello poskytovatele internetových služeb, který poskytuje rozsahu IP adres, nikoli vaším registrátorem názvu domény.</span><span class="sxs-lookup"><span data-stu-id="9321d-149">hello only difference is that you need tooconfigure hello name servers with hello ISP who provided your IP range, rather than your domain name registrar.</span></span>

## <a name="create-a-dns-ptr-record"></a><span data-ttu-id="9321d-150">Vytvořit záznam DNS PTR</span><span class="sxs-lookup"><span data-stu-id="9321d-150">Create a DNS PTR record</span></span>

### <a name="ipv4"></a><span data-ttu-id="9321d-151">IPv4</span><span class="sxs-lookup"><span data-stu-id="9321d-151">IPv4</span></span>

<span data-ttu-id="9321d-152">Hello následující příklad vás provede procesem hello vytvoření záznam PTR v zpětné zóny DNS v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-152">hello following example walks you through hello process of creating a PTR record in a reverse DNS zone in Azure DNS.</span></span> <span data-ttu-id="9321d-153">Pro jiné typy záznamů a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí portálu Azure hello](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-153">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

1.  <span data-ttu-id="9321d-154">Hello horní části hello **zónu DNS** vyberte **+ sady záznamů** tooopen hello **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="9321d-154">At hello top of hello **DNS zone** blade, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

 ![Zóna DNS](./media/dns-reverse-dns-hosting/figure4.png)

1. <span data-ttu-id="9321d-156">Na hello **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="9321d-156">On hello **Add record set** blade.</span></span> 
1. <span data-ttu-id="9321d-157">Vyberte **PTR** ze záznamu hello "**typ**" nabídky.</span><span class="sxs-lookup"><span data-stu-id="9321d-157">Select **PTR** from hello record "**Type**" menu.</span></span>  
1. <span data-ttu-id="9321d-158">Hello název sady záznamů hello pro záznam PTR musí toobe hello zbytek hello adresa IPv4 v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="9321d-158">hello name of hello record set for a PTR record needs toobe hello rest of hello IPv4 address in reverse order.</span></span> <span data-ttu-id="9321d-159">V tomto příkladu hello první tři oktety již naplní se jako součást název zóny hello (.2.0.192).</span><span class="sxs-lookup"><span data-stu-id="9321d-159">In this example, hello first three octets are already populated as part of hello zone name (.2.0.192).</span></span> <span data-ttu-id="9321d-160">Proto je pouze poslední oktet hello zadat v poli Název hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-160">Therefore, only hello last octet is supplied in hello name field.</span></span> <span data-ttu-id="9321d-161">Například mohla mít název vaší sady záznamů "**15**" pro prostředek, jehož IP adresa je 192.0.2.15.</span><span class="sxs-lookup"><span data-stu-id="9321d-161">For example, you could name your record set "**15**" for a resource whose IP address is 192.0.2.15.</span></span>  
1. <span data-ttu-id="9321d-162">V hello "**název domény**" Zadejte hello plně kvalifikovaný název domény (FQDN) hello prostředku pomocí hello IP.</span><span class="sxs-lookup"><span data-stu-id="9321d-162">In hello "**Domain Name**" field, enter hello fully qualified domain name (FQDN) of hello resource using hello IP.</span></span>
1. <span data-ttu-id="9321d-163">Vyberte **OK** dole hello záznam DNS hello okno toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-163">Select **OK** at hello bottom of hello blade toocreate hello DNS record.</span></span>

 ![Přidat sadu záznamů](./media/dns-reverse-dns-hosting/figure5.png)

<span data-ttu-id="9321d-165">Hello následuje několik příkladů, jak toocomplete tato úloha pomocí prostředí PowerShell a hello AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="9321d-165">hello following are examples on how toocomplete this task with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="9321d-166">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9321d-166">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a><span data-ttu-id="9321d-167">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9321d-167">AzureCLI 1.0</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a><span data-ttu-id="9321d-168">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9321d-168">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a><span data-ttu-id="9321d-169">IPv6</span><span class="sxs-lookup"><span data-stu-id="9321d-169">IPv6</span></span>

<span data-ttu-id="9321d-170">Hello následující ukázka vás provede procesem vytvoření nového záznamu, PTR, hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-170">hello following example walks you through hello process of creating new 'PTR' record.</span></span> <span data-ttu-id="9321d-171">Pro jiné typy záznamů a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí portálu Azure hello](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-171">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

1. <span data-ttu-id="9321d-172">Hello horní části hello **okno zóny DNS**, vyberte **+ sady záznamů** tooopen hello **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="9321d-172">At hello top of hello **DNS zone blade**, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

  ![okno zóny DNS](./media/dns-reverse-dns-hosting/figure6.png)

2. <span data-ttu-id="9321d-174">Na hello **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="9321d-174">On hello **Add record set** blade.</span></span> 
3. <span data-ttu-id="9321d-175">Vyberte **PTR** ze záznamu hello "**typ**" nabídky.</span><span class="sxs-lookup"><span data-stu-id="9321d-175">Select **PTR** from hello record "**Type**" menu.</span></span>  
4. <span data-ttu-id="9321d-176">Hello název sady záznamů hello pro záznam PTR musí toobe hello zbytek hello adresu IPv6 v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="9321d-176">hello name of hello record set for a PTR record needs toobe hello rest of hello IPv6 address in reverse order.</span></span> <span data-ttu-id="9321d-177">Nesmí obsahovat nula komprese.</span><span class="sxs-lookup"><span data-stu-id="9321d-177">It must not include any zero compression.</span></span> <span data-ttu-id="9321d-178">V tomto příkladu hello první 64bitová verze hello IPv6 jsou už vyplněné jako součást název zóny hello (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span><span class="sxs-lookup"><span data-stu-id="9321d-178">In this example, hello first 64 bits of hello IPv6 are already populated as part of hello zone name (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span></span> <span data-ttu-id="9321d-179">Proto se zadávají pouze hello posledních 64 bitů v poli Název hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-179">Therefore, only hello last 64 bits are supplied in hello name field.</span></span> <span data-ttu-id="9321d-180">Hello poslední 64 bitů hello IP adresy se zadávají v obráceném pořadí pomocí dobou jako oddělovače hello mezi každou hexadecimální číslo.</span><span class="sxs-lookup"><span data-stu-id="9321d-180">hello last 64 bits of hello IP address are entered in reverse order, using a period as hello delimiter between each hexadecimal number.</span></span> <span data-ttu-id="9321d-181">Například mohla mít název vaší sady záznamů "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" pro prostředek, jehož IP adresa je 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span><span class="sxs-lookup"><span data-stu-id="9321d-181">For example, you could name your record set "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" for a resource whose IP address is 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span></span>  
5. <span data-ttu-id="9321d-182">V hello "**název domény**" Zadejte hello plně kvalifikovaný název domény (FQDN) hello prostředku pomocí hello IP.</span><span class="sxs-lookup"><span data-stu-id="9321d-182">In hello "**Domain Name**" field, enter hello fully qualified domain name (FQDN) of hello resource using hello IP.</span></span>
6. <span data-ttu-id="9321d-183">Vyberte **OK** dole hello záznam DNS hello okno toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-183">Select **OK** at hello bottom of hello blade toocreate hello DNS record.</span></span>

![Přidat okno sady záznamů](./media/dns-reverse-dns-hosting/figure7.png)

<span data-ttu-id="9321d-185">Hello následuje několik příkladů, jak toocomplete tato úloha pomocí prostředí PowerShell a hello AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="9321d-185">hello following are examples on how toocomplete this task with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="9321d-186">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9321d-186">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a><span data-ttu-id="9321d-187">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9321d-187">AzureCLI 1.0</span></span>

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a><span data-ttu-id="9321d-188">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9321d-188">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a><span data-ttu-id="9321d-189">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="9321d-189">View Records</span></span>

<span data-ttu-id="9321d-190">tooview hello záznamy, které jste vytvořili, přejděte tooyour zónu DNS v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9321d-190">tooview hello records you created, navigate tooyour DNS zone in hello Azure portal.</span></span> <span data-ttu-id="9321d-191">V hello nižší součástí hello **zónu DNS** okně uvidíte hello záznamy pro zónu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="9321d-191">In hello lower part of hello **DNS zone** blade, you can see hello records for hello DNS zone.</span></span> <span data-ttu-id="9321d-192">Měli byste vidět hello výchozí NS a SOA záznamů, které jsou vytvořené v každé zóny, a všechny nové záznamy, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9321d-192">You should see hello default NS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

### <a name="ipv4"></a><span data-ttu-id="9321d-193">IPv4</span><span class="sxs-lookup"><span data-stu-id="9321d-193">IPv4</span></span>

<span data-ttu-id="9321d-194">Okno zóny DNS, zobrazují se záznamy IPv4 PTR:</span><span class="sxs-lookup"><span data-stu-id="9321d-194">DNS zone blade, showing IPv4 PTR records:</span></span>

![okno zóny DNS](./media/dns-reverse-dns-hosting/figure8.png)

<span data-ttu-id="9321d-196">Hello následující příklady ukazují, jak tooview hello PTR záznamy pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="9321d-196">hello following examples show how tooview hello PTR records using PowerShell or hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="9321d-197">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9321d-197">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="9321d-198">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9321d-198">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="9321d-199">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9321d-199">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="9321d-200">IPv6</span><span class="sxs-lookup"><span data-stu-id="9321d-200">IPv6</span></span>

<span data-ttu-id="9321d-201">Okno zóny DNS, zobrazují se záznamy IPv6 PTR:</span><span class="sxs-lookup"><span data-stu-id="9321d-201">DNS zone blade, showing IPv6 PTR records:</span></span>

![okno zóny DNS](./media/dns-reverse-dns-hosting/figure9.png)

<span data-ttu-id="9321d-203">Hello Následují příklady jak tooview hello záznamy pomocí prostředí PowerShell a hello AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="9321d-203">hello following are examples on how tooview hello records with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="9321d-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9321d-204">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="9321d-205">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9321d-205">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="9321d-206">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9321d-206">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a><span data-ttu-id="9321d-207">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="9321d-207">FAQ</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a><span data-ttu-id="9321d-208">Může hostovat zóny zpětného vyhledávání DNS pro moje bloky IP přiřazené poskytovatele internetových služeb ve službě Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="9321d-208">Can I host reverse DNS lookup zones for my ISP-assigned IP blocks on Azure DNS?</span></span>

<span data-ttu-id="9321d-209">Ano.</span><span class="sxs-lookup"><span data-stu-id="9321d-209">Yes.</span></span> <span data-ttu-id="9321d-210">Hostování hello zóny zpětného vyhledávání (ARPA) pro vlastní rozsahy IP v Azure DNS je plně podporována.</span><span class="sxs-lookup"><span data-stu-id="9321d-210">Hosting hello reverse lookup (ARPA) zones for your own IP ranges in Azure DNS is fully supported.</span></span>

<span data-ttu-id="9321d-211">Vytvořte zóny zpětného vyhledávání hello v Azure DNS jak je vysvětleno v tomto článku a pracovat s vašeho poskytovatele internetových služeb příliš[delegáta hello zóny](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-211">Create hello reverse lookup zone in Azure DNS as explained in this article, then work with your ISP too[delegate hello zone](dns-domain-delegation.md).</span></span>  <span data-ttu-id="9321d-212">Pak můžete spravovat hello záznamů PTR pro každý zpětného vyhledávání v hello stejným způsobem jako ostatní typy záznamů.</span><span class="sxs-lookup"><span data-stu-id="9321d-212">You can then manage hello PTR records for each reverse lookup in hello same way as other record types.</span></span>

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a><span data-ttu-id="9321d-213">Kolik nemá hostování Moje zpětné zóny náklady vyhledávání DNS?</span><span class="sxs-lookup"><span data-stu-id="9321d-213">How much does hosting my reverse DNS lookup zone cost?</span></span>

<span data-ttu-id="9321d-214">Hostování hello zpětné zóny vyhledávání DNS pro blok vašeho poskytovatele internetových služeb přiřadit IP v Azure DNS je účtován na [standardních sazeb Azure DNS](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="9321d-214">Hosting hello reverse DNS lookup zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a><span data-ttu-id="9321d-215">Může hostovat zpětné zóny vyhledávání DNS pro adresu IPv4 i IPv6 adresu v Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="9321d-215">Can I host reverse DNS lookup zones for both IPv4 and IPv6 addresses in Azure DNS?</span></span>

<span data-ttu-id="9321d-216">Ano.</span><span class="sxs-lookup"><span data-stu-id="9321d-216">Yes.</span></span> <span data-ttu-id="9321d-217">Tento článek vysvětluje, jak toocreate oba protokoly IPv4 a IPv6 DNS zóny zpětného vyhledávání v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-217">This article explains how toocreate both IPv4 and IPv6 reverse DNS lookup zones in Azure DNS.</span></span>

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a><span data-ttu-id="9321d-218">Můžete importovat existující zóně zpětného vyhledávání DNS?</span><span class="sxs-lookup"><span data-stu-id="9321d-218">Can I import an existing reverse DNS lookup zone?</span></span>

<span data-ttu-id="9321d-219">Ano.</span><span class="sxs-lookup"><span data-stu-id="9321d-219">Yes.</span></span> <span data-ttu-id="9321d-220">Můžete vytvořit hello rozhraní příkazového řádku Azure tooimport existující zóny DNS do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="9321d-220">You can use hello Azure CLI tooimport existing DNS zones into Azure DNS.</span></span> <span data-ttu-id="9321d-221">Tento postup funguje pro zóny dopředného vyhledávání a zóny zpětného vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="9321d-221">This works for both forward lookup zones and reverse lookup zones.</span></span>

<span data-ttu-id="9321d-222">Další informace najdete v tématu [Import a export souboru zóny DNS pomocí rozhraní příkazového řádku Azure hello](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-222">For more information, see [Import and export a DNS zone file using hello Azure CLI](dns-import-export.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9321d-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9321d-223">Next steps</span></span>

<span data-ttu-id="9321d-224">Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="9321d-224">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="9321d-225">Zjistěte, jak příliš[zpětné záznamy DNS pro služeb Azure spravovat](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="9321d-225">Learn how too[manage reverse DNS records for your Azure services](dns-reverse-dns-for-azure-services.md).</span></span>
