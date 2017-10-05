---
title: "Hostování DNS zóny zpětného vyhledávání v Azure DNS | Microsoft Docs"
description: "Naučte se používat Azure DNS pro hostování zpětné vyhledávání zóny DNS pro vaše rozsahy IP adres"
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
ms.openlocfilehash: 3e10b25d2f9b91c96af2958fef6dc6a4fdbff301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a><span data-ttu-id="8bd27-103">Hostování zpětné zóny DNS vyhledávání v Azure DNS</span><span class="sxs-lookup"><span data-stu-id="8bd27-103">Hosting reverse DNS lookup zones in Azure DNS</span></span>

<span data-ttu-id="8bd27-104">Tento článek vysvětluje, jak k hostování zpětné vyhledávání zóny DNS pro vaše přiřazené rozsahy IP v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-104">This article explains how to host the reverse DNS lookup zones for your assigned IP ranges in Azure DNS.</span></span> <span data-ttu-id="8bd27-105">Rozsahy IP reprezentována zóny zpětného vyhledávání musí přiřazené pro vaši organizaci, obvykle podle vašeho poskytovatele internetových služeb.</span><span class="sxs-lookup"><span data-stu-id="8bd27-105">The IP ranges represented by the reverse lookup zone must be assigned to your organization, typically by your ISP.</span></span>

<span data-ttu-id="8bd27-106">Konfigurace zpětné DNS pro Azure vlastní IP adresu přiřazenou k vaší službě Azure najdete v tématu [konfigurace zpětného vyhledávání pro IP adresy přidělené k službě Azure](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-106">To configure reverse DNS for Azure-owned IP address assigned to your Azure service, see [configure the reverse lookup for the IP addresses allocated to your Azure service](dns-reverse-dns-for-azure-services.md).</span></span>

<span data-ttu-id="8bd27-107">Před přečtení tohoto článku, měli byste se seznámit s tím [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-107">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="8bd27-108">Tento článek vás provede kroky k vytvoření vaší první zpětného vyhledávání DNS zóny a zaznamenejte pomocí portálu Azure, Azure PowerShell, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8bd27-108">This article walks you through the steps to create your first reverse lookup DNS zone and record using the Azure portal, Azure PowerShell, Azure CLI 1.0 or Azure CLI 2.0.</span></span>

## <a name="create-a-reverse-lookup-dns-zone"></a><span data-ttu-id="8bd27-109">Vytvořit zónu zpětného vyhledávání DNS</span><span class="sxs-lookup"><span data-stu-id="8bd27-109">Create a reverse lookup DNS zone</span></span>

1. <span data-ttu-id="8bd27-110">Přihlaste se k [portálu Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="8bd27-110">Sign in to the [Azure portal](https://portal.azure.com)</span></span>
1. <span data-ttu-id="8bd27-111">V nabídce centra klikněte a klikněte na **nový** > **sítě** > a pak klikněte na **zónu DNS** otevřete **zóny DNS vytvořte** okno.</span><span class="sxs-lookup"><span data-stu-id="8bd27-111">On the Hub menu, click and click **New** > **Networking** > and then click **DNS zone** to open the **Create DNS zone** blade.</span></span>

   ![Zóna DNS](./media/dns-reverse-dns-hosting/figure1.png)

1. <span data-ttu-id="8bd27-113">Na **zóny DNS vytvořte** okně název zóny DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-113">On the **Create DNS zone** blade, name your DNS zone.</span></span> <span data-ttu-id="8bd27-114">Název zóny je jinak vytvořených pro předpony IPv4 a IPv6.</span><span class="sxs-lookup"><span data-stu-id="8bd27-114">The name of the zone is crafted differently for IPv4 and IPv6 prefixes.</span></span> <span data-ttu-id="8bd27-115">Použít pokyny v [IPV4](#ipv4) nebo [IPv6](#ipv6) na název zóny.</span><span class="sxs-lookup"><span data-stu-id="8bd27-115">Use either the instructions for [IPV4](#ipv4) or [IPv6](#ipv6) to name your zone.</span></span> <span data-ttu-id="8bd27-116">Po dokončení klikněte na tlačítko **vytvořit** k vytvoření zóny.</span><span class="sxs-lookup"><span data-stu-id="8bd27-116">When complete click **Create** to create the zone.</span></span>

### <a name="ipv4"></a><span data-ttu-id="8bd27-117">IPv4</span><span class="sxs-lookup"><span data-stu-id="8bd27-117">IPv4</span></span>

<span data-ttu-id="8bd27-118">Název zóny zpětného vyhledávání IPv4 je založen na rozsah IP, který představuje.</span><span class="sxs-lookup"><span data-stu-id="8bd27-118">The name of an IPv4 reverse lookup zone is based on the IP range it represents.</span></span> <span data-ttu-id="8bd27-119">Musí být v následujícím formátu: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="8bd27-119">It should be in the following format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span></span> <span data-ttu-id="8bd27-120">Příklady najdete v tématu [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md#ipv4).</span><span class="sxs-lookup"><span data-stu-id="8bd27-120">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv4).</span></span>

> [!NOTE]
> <span data-ttu-id="8bd27-121">Při vytváření classless zóny zpětného vyhledávání DNS v Azure DNS, je nutné použít pomlčka (`-`) namísto lomítkem (/') v názvu zóny.</span><span class="sxs-lookup"><span data-stu-id="8bd27-121">When creating classless reverse DNS lookup zones in Azure DNS, you must use a hyphen (`-`) rather than a forward slash ('/') in the zone name.</span></span>
>
> <span data-ttu-id="8bd27-122">Například pro 192.0.2.128/26 rozsah IP, musíte použít `128-26.2.0.192.in-addr.arpa` jako název zóny místo `128/26.2.0.192.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="8bd27-122">For example, for the IP range 192.0.2.128/26, you must use `128-26.2.0.192.in-addr.arpa` as the zone name instead of `128/26.2.0.192.in-addr.arpa`.</span></span>
>
> <span data-ttu-id="8bd27-123">Důvodem je, že když jsou oba podporované ve standardech DNS, zón služby DNS názvy obsahující lomítko (`/`) znak nejsou podporovány v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-123">This is because, while both are supported by the DNS standards, DNS zone names containing the forward slash (`/`) character are not supported in Azure DNS.</span></span>

<span data-ttu-id="8bd27-124">Následující příklad ukazuje, jak vytvořit "Třída C" zpětné zóny DNS s názvem `2.0.192.in-addr.arpa` v Azure DNS prostřednictvím portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="8bd27-124">The following example shows how to create a 'Class C' reverse DNS zone named `2.0.192.in-addr.arpa` in Azure DNS via the Azure portal:</span></span>

 ![Vytvoření zóny DNS](./media/dns-reverse-dns-hosting/figure2.png)

<span data-ttu-id="8bd27-126">Umístění skupiny prostředků definuje umístění pro skupinu prostředků a nemá žádný vliv na zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-126">The 'Resource group location' defines the location for the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="8bd27-127">Umístění zóny DNS je vždy "globální a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="8bd27-127">The DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="8bd27-128">Následující příklady ukazují, jak pro dokončení této úlohy Azure PowerShell a rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="8bd27-128">The following examples show how to complete this task with Azure PowerShell and the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8bd27-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd27-129">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="8bd27-130">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-130">Azure CLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="8bd27-131">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-131">Azure CLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="8bd27-132">IPv6</span><span class="sxs-lookup"><span data-stu-id="8bd27-132">IPv6</span></span>

<span data-ttu-id="8bd27-133">Název zóny zpětného vyhledávání IPv6 musí být v následující podobě: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span><span class="sxs-lookup"><span data-stu-id="8bd27-133">The name of an IPv6 reverse lookup zone should be in the following form: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span></span>  <span data-ttu-id="8bd27-134">Příklady najdete v tématu [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md#ipv6).</span><span class="sxs-lookup"><span data-stu-id="8bd27-134">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv6).</span></span>


<span data-ttu-id="8bd27-135">Následující příklad ukazuje, jak vytvořit IPv6 zpětného vyhledávání zónu DNS s názvem `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` v Azure DNS prostřednictvím portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="8bd27-135">The following example shows how to create an IPv6 reverse DNS lookup zone named `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via the Azure portal:</span></span>

 ![Vytvoření zóny DNS](./media/dns-reverse-dns-hosting/figure3.png)

<span data-ttu-id="8bd27-137">Umístění skupiny prostředků definuje umístění pro skupinu prostředků a nemá žádný vliv na zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-137">The 'Resource group location' defines the location for the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="8bd27-138">Umístění zóny DNS je vždy "globální a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="8bd27-138">The DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="8bd27-139">Následující příklady ukazují, jak pro dokončení této úlohy Azure PowerShell a rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="8bd27-139">The following examples show how to complete this task with Azure PowerShell and the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8bd27-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd27-140">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a><span data-ttu-id="8bd27-141">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-141">AzureCLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a><span data-ttu-id="8bd27-142">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-142">AzureCLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a><span data-ttu-id="8bd27-143">Delegování zóny zpětného vyhledávání DNS</span><span class="sxs-lookup"><span data-stu-id="8bd27-143">Delegate a reverse DNS lookup zone</span></span>

<span data-ttu-id="8bd27-144">Vytvoření zóny zpětného vyhledávání DNS je nutné zajistit, že je zóna delegovaná z nadřazené zóně.</span><span class="sxs-lookup"><span data-stu-id="8bd27-144">Having created your reverse DNS lookup zone, you must ensure that the zone is delegated from the parent zone.</span></span> <span data-ttu-id="8bd27-145">Delegování DNS umožňuje proces překladu názvů DNS najít názvové servery hostující vaše zóny zpětného vyhledávání DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-145">DNS delegation enables the DNS name resolution process to find the name servers hosting your reverse DNS lookup zone.</span></span> <span data-ttu-id="8bd27-146">To umožňuje tyto názvové servery k zodpovězení zpětné dotazy DNS pro IP adresy ve vaší rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="8bd27-146">This enables those name servers to answer DNS reverse queries for the IP addresses in your address range.</span></span>

<span data-ttu-id="8bd27-147">Pro zóny dopředného vyhledávání, je popsán proces delegování zóny DNS v [delegování domény do Azure DNS](dns-delegate-domain-azure-dns.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-147">For forward lookup zones, the process of delegating a DNS zone is described in [Delegate your domain to Azure DNS](dns-delegate-domain-azure-dns.md).</span></span> <span data-ttu-id="8bd27-148">Delegování zón zpětného vyhledávání funguje stejným způsobem.</span><span class="sxs-lookup"><span data-stu-id="8bd27-148">Delegation for reverse lookup zones works the same way.</span></span> <span data-ttu-id="8bd27-149">Jediným rozdílem je, že budete muset nakonfigurovat názvové servery s poskytovatele internetových služeb, který poskytuje rozsahu IP adres, nikoli vaším registrátorem názvu domény.</span><span class="sxs-lookup"><span data-stu-id="8bd27-149">The only difference is that you need to configure the name servers with the ISP who provided your IP range, rather than your domain name registrar.</span></span>

## <a name="create-a-dns-ptr-record"></a><span data-ttu-id="8bd27-150">Vytvořit záznam DNS PTR</span><span class="sxs-lookup"><span data-stu-id="8bd27-150">Create a DNS PTR record</span></span>

### <a name="ipv4"></a><span data-ttu-id="8bd27-151">IPv4</span><span class="sxs-lookup"><span data-stu-id="8bd27-151">IPv4</span></span>

<span data-ttu-id="8bd27-152">Následující příklad vás provede procesem vytvoření záznam PTR v zpětné zóny DNS v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-152">The following example walks you through the process of creating a PTR record in a reverse DNS zone in Azure DNS.</span></span> <span data-ttu-id="8bd27-153">Informace o dalších typech záznamů a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-153">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

1.  <span data-ttu-id="8bd27-154">V horní části okna **Zóna DNS** vyberte **Sada záznamů**. Otevře se okno **Přidat sadu záznamů**.</span><span class="sxs-lookup"><span data-stu-id="8bd27-154">At the top of the **DNS zone** blade, select **+ Record set** to open the **Add record set** blade.</span></span>

 ![Zóna DNS](./media/dns-reverse-dns-hosting/figure4.png)

1. <span data-ttu-id="8bd27-156">Na **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="8bd27-156">On the **Add record set** blade.</span></span> 
1. <span data-ttu-id="8bd27-157">Vyberte **PTR** ze záznamu "**typ**" nabídky.</span><span class="sxs-lookup"><span data-stu-id="8bd27-157">Select **PTR** from the record "**Type**" menu.</span></span>  
1. <span data-ttu-id="8bd27-158">Název sady pro záznam PTR záznamů musí být zbytek adresy IPv4 v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="8bd27-158">The name of the record set for a PTR record needs to be the rest of the IPv4 address in reverse order.</span></span> <span data-ttu-id="8bd27-159">V tomto příkladu jsou první tři oktety už vyplněné jako součást název zóny (.2.0.192).</span><span class="sxs-lookup"><span data-stu-id="8bd27-159">In this example, the first three octets are already populated as part of the zone name (.2.0.192).</span></span> <span data-ttu-id="8bd27-160">Proto je pouze poslední oktet zadaný v poli název.</span><span class="sxs-lookup"><span data-stu-id="8bd27-160">Therefore, only the last octet is supplied in the name field.</span></span> <span data-ttu-id="8bd27-161">Například mohla mít název vaší sady záznamů "**15**" pro prostředek, jehož IP adresa je 192.0.2.15.</span><span class="sxs-lookup"><span data-stu-id="8bd27-161">For example, you could name your record set "**15**" for a resource whose IP address is 192.0.2.15.</span></span>  
1. <span data-ttu-id="8bd27-162">V "**název domény**" pole, zadejte plně kvalifikovaný název domény (FQDN) prostředku pomocí IP.</span><span class="sxs-lookup"><span data-stu-id="8bd27-162">In the "**Domain Name**" field, enter the fully qualified domain name (FQDN) of the resource using the IP.</span></span>
1. <span data-ttu-id="8bd27-163">Vytvořte záznam DNS výběrem **OK** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="8bd27-163">Select **OK** at the bottom of the blade to create the DNS record.</span></span>

 ![Přidat sadu záznamů](./media/dns-reverse-dns-hosting/figure5.png)

<span data-ttu-id="8bd27-165">Následují příklady o tom, jak pomocí prostředí PowerShell a AzureCLI tuto úlohu dokončit:</span><span class="sxs-lookup"><span data-stu-id="8bd27-165">The following are examples on how to complete this task with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8bd27-166">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd27-166">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a><span data-ttu-id="8bd27-167">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-167">AzureCLI 1.0</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a><span data-ttu-id="8bd27-168">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-168">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a><span data-ttu-id="8bd27-169">IPv6</span><span class="sxs-lookup"><span data-stu-id="8bd27-169">IPv6</span></span>

<span data-ttu-id="8bd27-170">Následující příklad vás provede procesem vytvoření nového záznamu, PTR'.</span><span class="sxs-lookup"><span data-stu-id="8bd27-170">The following example walks you through the process of creating new 'PTR' record.</span></span> <span data-ttu-id="8bd27-171">Informace o dalších typech záznamů a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-171">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

1. <span data-ttu-id="8bd27-172">V horní části **okno zóny DNS**, vyberte **+ sady záznamů** otevřete **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="8bd27-172">At the top of the **DNS zone blade**, select **+ Record set** to open the **Add record set** blade.</span></span>

  ![okno zóny DNS](./media/dns-reverse-dns-hosting/figure6.png)

2. <span data-ttu-id="8bd27-174">Na **přidat sadu záznamů** okno.</span><span class="sxs-lookup"><span data-stu-id="8bd27-174">On the **Add record set** blade.</span></span> 
3. <span data-ttu-id="8bd27-175">Vyberte **PTR** ze záznamu "**typ**" nabídky.</span><span class="sxs-lookup"><span data-stu-id="8bd27-175">Select **PTR** from the record "**Type**" menu.</span></span>  
4. <span data-ttu-id="8bd27-176">Název sady pro záznam PTR záznamů musí být zbytek adresu IPv6 v obráceném pořadí.</span><span class="sxs-lookup"><span data-stu-id="8bd27-176">The name of the record set for a PTR record needs to be the rest of the IPv6 address in reverse order.</span></span> <span data-ttu-id="8bd27-177">Nesmí obsahovat nula komprese.</span><span class="sxs-lookup"><span data-stu-id="8bd27-177">It must not include any zero compression.</span></span> <span data-ttu-id="8bd27-178">V tomto příkladu jsou už vyplněné prvních 64 bitů IPv6 v rámci název zóny (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span><span class="sxs-lookup"><span data-stu-id="8bd27-178">In this example, the first 64 bits of the IPv6 are already populated as part of the zone name (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span></span> <span data-ttu-id="8bd27-179">Proto se v poli Název zadávají pouze posledních 64 bitů.</span><span class="sxs-lookup"><span data-stu-id="8bd27-179">Therefore, only the last 64 bits are supplied in the name field.</span></span> <span data-ttu-id="8bd27-180">Posledních 64 bitů IP adresy se zadávají v obráceném pořadí použití tečky jako oddělovač mezi každou hexadecimální číslo.</span><span class="sxs-lookup"><span data-stu-id="8bd27-180">The last 64 bits of the IP address are entered in reverse order, using a period as the delimiter between each hexadecimal number.</span></span> <span data-ttu-id="8bd27-181">Například mohla mít název vaší sady záznamů "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" pro prostředek, jehož IP adresa je 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span><span class="sxs-lookup"><span data-stu-id="8bd27-181">For example, you could name your record set "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" for a resource whose IP address is 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span></span>  
5. <span data-ttu-id="8bd27-182">V "**název domény**" pole, zadejte plně kvalifikovaný název domény (FQDN) prostředku pomocí IP.</span><span class="sxs-lookup"><span data-stu-id="8bd27-182">In the "**Domain Name**" field, enter the fully qualified domain name (FQDN) of the resource using the IP.</span></span>
6. <span data-ttu-id="8bd27-183">Vytvořte záznam DNS výběrem **OK** v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="8bd27-183">Select **OK** at the bottom of the blade to create the DNS record.</span></span>

![Přidat okno sady záznamů](./media/dns-reverse-dns-hosting/figure7.png)

<span data-ttu-id="8bd27-185">Následují příklady o tom, jak pomocí prostředí PowerShell a AzureCLI tuto úlohu dokončit:</span><span class="sxs-lookup"><span data-stu-id="8bd27-185">The following are examples on how to complete this task with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8bd27-186">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd27-186">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a><span data-ttu-id="8bd27-187">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-187">AzureCLI 1.0</span></span>

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a><span data-ttu-id="8bd27-188">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-188">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a><span data-ttu-id="8bd27-189">Zobrazení záznamů</span><span class="sxs-lookup"><span data-stu-id="8bd27-189">View Records</span></span>

<span data-ttu-id="8bd27-190">Pokud chcete zobrazit záznamy, které jste vytvořili, přejděte do zóny DNS na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8bd27-190">To view the records you created, navigate to your DNS zone in the Azure portal.</span></span> <span data-ttu-id="8bd27-191">V dolní části **zónu DNS** okně uvidíte záznamy pro zónu DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-191">In the lower part of the **DNS zone** blade, you can see the records for the DNS zone.</span></span> <span data-ttu-id="8bd27-192">Měli byste vidět výchozí záznamy NS a SOA, které se vytvoří v každé zóně, a nové záznamy, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="8bd27-192">You should see the default NS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

### <a name="ipv4"></a><span data-ttu-id="8bd27-193">IPv4</span><span class="sxs-lookup"><span data-stu-id="8bd27-193">IPv4</span></span>

<span data-ttu-id="8bd27-194">Okno zóny DNS, zobrazují se záznamy IPv4 PTR:</span><span class="sxs-lookup"><span data-stu-id="8bd27-194">DNS zone blade, showing IPv4 PTR records:</span></span>

![okno zóny DNS](./media/dns-reverse-dns-hosting/figure8.png)

<span data-ttu-id="8bd27-196">Následující příklady ukazují, jak zobrazit záznamů PTR pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="8bd27-196">The following examples show how to view the PTR records using PowerShell or the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8bd27-197">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd27-197">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="8bd27-198">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-198">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="8bd27-199">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-199">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="8bd27-200">IPv6</span><span class="sxs-lookup"><span data-stu-id="8bd27-200">IPv6</span></span>

<span data-ttu-id="8bd27-201">Okno zóny DNS, zobrazují se záznamy IPv6 PTR:</span><span class="sxs-lookup"><span data-stu-id="8bd27-201">DNS zone blade, showing IPv6 PTR records:</span></span>

![okno zóny DNS](./media/dns-reverse-dns-hosting/figure9.png)

<span data-ttu-id="8bd27-203">Následují příklady o tom, jak zobrazit záznamy pomocí prostředí PowerShell a AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="8bd27-203">The following are examples on how to view the records with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8bd27-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd27-204">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="8bd27-205">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-205">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="8bd27-206">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8bd27-206">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a><span data-ttu-id="8bd27-207">Nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="8bd27-207">FAQ</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a><span data-ttu-id="8bd27-208">Může hostovat zóny zpětného vyhledávání DNS pro moje bloky IP přiřazené poskytovatele internetových služeb ve službě Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="8bd27-208">Can I host reverse DNS lookup zones for my ISP-assigned IP blocks on Azure DNS?</span></span>

<span data-ttu-id="8bd27-209">Ano.</span><span class="sxs-lookup"><span data-stu-id="8bd27-209">Yes.</span></span> <span data-ttu-id="8bd27-210">Hostování zóny zpětného vyhledávání (ARPA) pro vlastní rozsahy IP v Azure DNS je plně podporována.</span><span class="sxs-lookup"><span data-stu-id="8bd27-210">Hosting the reverse lookup (ARPA) zones for your own IP ranges in Azure DNS is fully supported.</span></span>

<span data-ttu-id="8bd27-211">Vytvořit zóny zpětného vyhledávání v Azure DNS, jak je vysvětleno v tomto článku, pak fungovat u svého poskytovatele [delegování zóny](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-211">Create the reverse lookup zone in Azure DNS as explained in this article, then work with your ISP to [delegate the zone](dns-domain-delegation.md).</span></span>  <span data-ttu-id="8bd27-212">Pak můžete spravovat záznamů PTR pro každý zpětného vyhledávání stejným způsobem jako jiné typy záznamů.</span><span class="sxs-lookup"><span data-stu-id="8bd27-212">You can then manage the PTR records for each reverse lookup in the same way as other record types.</span></span>

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a><span data-ttu-id="8bd27-213">Kolik nemá hostování Moje zpětné zóny náklady vyhledávání DNS?</span><span class="sxs-lookup"><span data-stu-id="8bd27-213">How much does hosting my reverse DNS lookup zone cost?</span></span>

<span data-ttu-id="8bd27-214">Hostování zóny zpětného vyhledávání DNS pro blok vašeho poskytovatele internetových služeb přiřadit IP v Azure DNS je účtován na [standardních sazeb Azure DNS](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="8bd27-214">Hosting the reverse DNS lookup zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a><span data-ttu-id="8bd27-215">Může hostovat zpětné zóny vyhledávání DNS pro adresu IPv4 i IPv6 adresu v Azure DNS?</span><span class="sxs-lookup"><span data-stu-id="8bd27-215">Can I host reverse DNS lookup zones for both IPv4 and IPv6 addresses in Azure DNS?</span></span>

<span data-ttu-id="8bd27-216">Ano.</span><span class="sxs-lookup"><span data-stu-id="8bd27-216">Yes.</span></span> <span data-ttu-id="8bd27-217">Tento článek vysvětluje, jak vytvořit oba protokoly IPv4 a IPv6 zpětné zóny vyhledávání DNS v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-217">This article explains how to create both IPv4 and IPv6 reverse DNS lookup zones in Azure DNS.</span></span>

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a><span data-ttu-id="8bd27-218">Můžete importovat existující zóně zpětného vyhledávání DNS?</span><span class="sxs-lookup"><span data-stu-id="8bd27-218">Can I import an existing reverse DNS lookup zone?</span></span>

<span data-ttu-id="8bd27-219">Ano.</span><span class="sxs-lookup"><span data-stu-id="8bd27-219">Yes.</span></span> <span data-ttu-id="8bd27-220">Rozhraní příkazového řádku Azure můžete importovat existující zóny DNS do Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="8bd27-220">You can use the Azure CLI to import existing DNS zones into Azure DNS.</span></span> <span data-ttu-id="8bd27-221">Tento postup funguje pro zóny dopředného vyhledávání a zóny zpětného vyhledávání.</span><span class="sxs-lookup"><span data-stu-id="8bd27-221">This works for both forward lookup zones and reverse lookup zones.</span></span>

<span data-ttu-id="8bd27-222">Další informace najdete v tématu [Import a export souboru zóny DNS pomocí rozhraní příkazového řádku Azure](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-222">For more information, see [Import and export a DNS zone file using the Azure CLI](dns-import-export.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8bd27-223">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8bd27-223">Next steps</span></span>

<span data-ttu-id="8bd27-224">Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="8bd27-224">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="8bd27-225">Zjistěte, jak [zpětné záznamy DNS pro služeb Azure spravovat](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="8bd27-225">Learn how to [manage reverse DNS records for your Azure services](dns-reverse-dns-for-azure-services.md).</span></span>
