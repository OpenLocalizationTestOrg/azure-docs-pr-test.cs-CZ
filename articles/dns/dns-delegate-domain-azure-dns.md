---
title: "aaaDelegate tooAzure vaší domény DNS | Microsoft Docs"
description: "Pochopte, jak se toochange delegování domény a použití Azure DNS název, hostování domény tooprovide servery."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: f780bdaa416150e5e3afe6c6845dc75ba54b6203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-a-domain-tooazure-dns"></a><span data-ttu-id="27bb1-103">Delegát tooAzure domény DNS</span><span class="sxs-lookup"><span data-stu-id="27bb1-103">Delegate a domain tooAzure DNS</span></span>

<span data-ttu-id="27bb1-104">Azure DNS vám umožní toohost zóny DNS a spravovat hello záznamy DNS pro doménu v Azure.</span><span class="sxs-lookup"><span data-stu-id="27bb1-104">Azure DNS allows you toohost a DNS zone and manage hello DNS records for a domain in Azure.</span></span> <span data-ttu-id="27bb1-105">Aby dotazy DNS pro domény tooreach Azure DNS, má doména hello toobe delegovaná z nadřazené domény hello tooAzure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-105">In order for DNS queries for a domain tooreach Azure DNS, hello domain has toobe delegated tooAzure DNS from hello parent domain.</span></span> <span data-ttu-id="27bb1-106">Mějte na paměti Azure DNS není doménový Registrátor hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-106">Keep in mind Azure DNS is not hello domain registrar.</span></span> <span data-ttu-id="27bb1-107">Tento článek vysvětluje, jak toodelegate tooAzure vaší domény DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-107">This article explains how toodelegate your domain tooAzure DNS.</span></span>

<span data-ttu-id="27bb1-108">U domén zakoupených od doménového registrátora registrátora nabízí možnost tooset hello si tyto záznamy NS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-108">For domains purchased from a registrar, your registrar offers hello option tooset up these NS records.</span></span> <span data-ttu-id="27bb1-109">Nemáte tooown domény toocreate zónu DNS s tímto názvem domény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-109">You do not have tooown a domain toocreate a DNS zone with that domain name in Azure DNS.</span></span> <span data-ttu-id="27bb1-110">Je však nutné, aby tooown hello domény tooset až tooAzure hello delegování DNS u registrátora hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-110">However, you do need tooown hello domain tooset up hello delegation tooAzure DNS with hello registrar.</span></span>

<span data-ttu-id="27bb1-111">Předpokládejme například, nákupu hello domény "contoso.net" a v Azure DNS vytvoříte zónu s názvem hello "contoso.net".</span><span class="sxs-lookup"><span data-stu-id="27bb1-111">For example, suppose you purchase hello domain 'contoso.net' and create a zone with hello name 'contoso.net' in Azure DNS.</span></span> <span data-ttu-id="27bb1-112">Jako vlastník hello hello domény vašeho registrátora nabízí že Hello možnost tooconfigure hello adresy názvových serverů (tedy záznamy hello NS) pro doménu.</span><span class="sxs-lookup"><span data-stu-id="27bb1-112">As hello owner of hello domain, your registrar offers you hello option tooconfigure hello name server addresses (that is, hello NS records) for your domain.</span></span> <span data-ttu-id="27bb1-113">Hello Registrátor uloží tyto záznamy NS v nadřazené doméně hello, v tomto případě ".net..</span><span class="sxs-lookup"><span data-stu-id="27bb1-113">hello registrar stores these NS records in hello parent domain, in this case '.net'.</span></span> <span data-ttu-id="27bb1-114">Klienti kolem hello, world pak lze směrovanou tooyour doménu v zóně Azure DNS, při pokusu o tooresolve záznamů DNS v "contoso.net".</span><span class="sxs-lookup"><span data-stu-id="27bb1-114">Clients around hello world can then be directed tooyour domain in Azure DNS zone when trying tooresolve DNS records in 'contoso.net'.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="27bb1-115">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="27bb1-115">Create a DNS zone</span></span>

1. <span data-ttu-id="27bb1-116">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="27bb1-116">Sign in toohello Azure portal</span></span>
1. <span data-ttu-id="27bb1-117">V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.</span><span class="sxs-lookup"><span data-stu-id="27bb1-117">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="27bb1-119">Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="27bb1-119">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>

   | <span data-ttu-id="27bb1-120">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="27bb1-120">**Setting**</span></span> | <span data-ttu-id="27bb1-121">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="27bb1-121">**Value**</span></span> | <span data-ttu-id="27bb1-122">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="27bb1-122">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="27bb1-123">**Název**</span><span class="sxs-lookup"><span data-stu-id="27bb1-123">**Name**</span></span>|<span data-ttu-id="27bb1-124">contoso.net</span><span class="sxs-lookup"><span data-stu-id="27bb1-124">contoso.net</span></span>|<span data-ttu-id="27bb1-125">Název zóny DNS hello Hello</span><span class="sxs-lookup"><span data-stu-id="27bb1-125">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="27bb1-126">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="27bb1-126">**Subscription**</span></span>|<span data-ttu-id="27bb1-127">[Vaše předplatné]</span><span class="sxs-lookup"><span data-stu-id="27bb1-127">[Your subscription]</span></span>|<span data-ttu-id="27bb1-128">Vyberte bránu předplatné toocreate hello aplikace v.</span><span class="sxs-lookup"><span data-stu-id="27bb1-128">Select a subscription toocreate hello application gateway in.</span></span>|
   |<span data-ttu-id="27bb1-129">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="27bb1-129">**Resource group**</span></span>|<span data-ttu-id="27bb1-130">**Vytvořit novou:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="27bb1-130">**Create new:** contosoRG</span></span>|<span data-ttu-id="27bb1-131">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="27bb1-131">Create a resource group.</span></span> <span data-ttu-id="27bb1-132">Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="27bb1-132">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="27bb1-133">Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.</span><span class="sxs-lookup"><span data-stu-id="27bb1-133">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="27bb1-134">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="27bb1-134">**Location**</span></span>|<span data-ttu-id="27bb1-135">Západní USA</span><span class="sxs-lookup"><span data-stu-id="27bb1-135">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="27bb1-136">Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-136">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="27bb1-137">umístění zóny DNS Hello je vždy "globální" a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="27bb1-137">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="retrieve-name-servers"></a><span data-ttu-id="27bb1-138">Načtení názvových serverů</span><span class="sxs-lookup"><span data-stu-id="27bb1-138">Retrieve name servers</span></span>

<span data-ttu-id="27bb1-139">Předtím, než můžete delegovat vaše tooAzure DNS zóny DNS, musíte nejprve názvy tooknow hello názvových serverů zóny.</span><span class="sxs-lookup"><span data-stu-id="27bb1-139">Before you can delegate your DNS zone tooAzure DNS, you first need tooknow hello name server names for your zone.</span></span> <span data-ttu-id="27bb1-140">Azure DNS přiděluje názvové servery z fondu vždy, když je vytvořena zóna.</span><span class="sxs-lookup"><span data-stu-id="27bb1-140">Azure DNS allocates name servers from a pool each time a zone is created.</span></span>

1. <span data-ttu-id="27bb1-141">S zóny DNS hello vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="27bb1-141">With hello DNS zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="27bb1-142">Klikněte na tlačítko hello **contoso.net** zónu DNS v hello **všechny prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="27bb1-142">Click hello **contoso.net** DNS zone in hello **All resources** blade.</span></span> <span data-ttu-id="27bb1-143">Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **contoso.net** v hello filtr podle názvu...</span><span class="sxs-lookup"><span data-stu-id="27bb1-143">If hello subscription you selected already has several resources in it, you can enter **contoso.net** in hello Filter by name…</span></span> <span data-ttu-id="27bb1-144">pole tooeasily přístup hello aplikační brány.</span><span class="sxs-lookup"><span data-stu-id="27bb1-144">box tooeasily access hello application gateway.</span></span> 

1. <span data-ttu-id="27bb1-145">V okně zóny DNS hello načíst hello názvové servery.</span><span class="sxs-lookup"><span data-stu-id="27bb1-145">Retrieve hello name servers from hello DNS zone blade.</span></span> <span data-ttu-id="27bb1-146">V tomto příkladu hello zónu "contoso.net" přiřazené názvové servery ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS.NET', ' ns3-01.azure-dns.org', a ' ns4-01.azure-dns.info':</span><span class="sxs-lookup"><span data-stu-id="27bb1-146">In this example, hello zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="27bb1-148">Azure DNS automaticky vytvoří záznamy autoritativních NS ve vaší zóně obsahující hello přiřazené názvové servery.</span><span class="sxs-lookup"><span data-stu-id="27bb1-148">Azure DNS automatically creates authoritative NS records in your zone containing hello assigned name servers.</span></span>  <span data-ttu-id="27bb1-149">názvy toosee hello název serveru pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure, jednoduše musíte tooretrieve tyto záznamy.</span><span class="sxs-lookup"><span data-stu-id="27bb1-149">toosee hello name server names via Azure PowerShell or Azure CLI, you simply need tooretrieve these records.</span></span>

<span data-ttu-id="27bb1-150">Hello následující příklady také popisují hello tooretrieve hello názvové servery pro zónu v Azure DNS pomocí prostředí PowerShell a rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="27bb1-150">hello following examples also provide hello steps tooretrieve hello name servers for a zone in Azure DNS with PowerShell and Azure CLI.</span></span>

### <a name="powershell"></a><span data-ttu-id="27bb1-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27bb1-151">PowerShell</span></span>

```powershell
# hello record name "@" is used toorefer toorecords at hello top of hello zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

<span data-ttu-id="27bb1-152">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="27bb1-152">hello following example is hello response.</span></span>

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a><span data-ttu-id="27bb1-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="27bb1-153">Azure CLI</span></span>

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

<span data-ttu-id="27bb1-154">Následující ukázka Hello je hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="27bb1-154">hello following example is hello response.</span></span>

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-hello-domain"></a><span data-ttu-id="27bb1-155">Delegát hello domény</span><span class="sxs-lookup"><span data-stu-id="27bb1-155">Delegate hello domain</span></span>

<span data-ttu-id="27bb1-156">Teď, když je vytvořena zóna DNS hello a máte hello názvové servery, musí hello nadřazené domény toobe aktualizovat pomocí názvových serverů Azure DNS hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-156">Now that hello DNS zone is created and you have hello name servers, hello parent domain needs toobe updated with hello Azure DNS name servers.</span></span> <span data-ttu-id="27bb1-157">Každý Registrátor má vlastní DNS správy nástroje toochange hello záznamy názvového serveru pro doménu.</span><span class="sxs-lookup"><span data-stu-id="27bb1-157">Each registrar has their own DNS management tools toochange hello name server records for a domain.</span></span> <span data-ttu-id="27bb1-158">Na stránce správy DNS vašeho registrátora hello upravit záznamy NS hello a nahraďte záznamy NS hello hello ty, které vytvořil Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-158">In hello registrar's DNS management page, edit hello NS records and replace hello NS records with hello ones Azure DNS created.</span></span>

<span data-ttu-id="27bb1-159">Při delegování domény tooAzure DNS, musíte použít názvy názvových serverů hello poskytuje Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-159">When delegating a domain tooAzure DNS, you must use hello name server names provided by Azure DNS.</span></span> <span data-ttu-id="27bb1-160">Je doporučeno toouse všechny čtyři název názvů serverů, bez ohledu na to hello název vaší domény.</span><span class="sxs-lookup"><span data-stu-id="27bb1-160">It is recommended toouse all four name server names, regardless of hello name of your domain.</span></span> <span data-ttu-id="27bb1-161">Delegování domény nevyžaduje hello název serveru název toouse hello stejné domény nejvyšší úrovně jako doménu.</span><span class="sxs-lookup"><span data-stu-id="27bb1-161">Domain delegation does not require hello name server name toouse hello same top-level domain as your domain.</span></span>

<span data-ttu-id="27bb1-162">Neměli byste používat "spojovací záznamy" toopoint toohello Azure DNS název IP adresy serverů, protože tyto IP adresy mohou v budoucnu měnit.</span><span class="sxs-lookup"><span data-stu-id="27bb1-162">You should not use 'glue records' toopoint toohello Azure DNS name server IP addresses, since these IP addresses may change in future.</span></span> <span data-ttu-id="27bb1-163">Delegování pomocí názvů názvových serverů ve vaší vlastní zóně, někdy označovaných jako „jednoduché názvové servery“, v současné době není v Azure DNS podporované.</span><span class="sxs-lookup"><span data-stu-id="27bb1-163">Delegations using name server names in your own zone, sometimes called 'vanity name servers', are not currently supported in Azure DNS.</span></span>

## <a name="verify-name-resolution-is-working"></a><span data-ttu-id="27bb1-164">Ověření, že překlad názvů funguje</span><span class="sxs-lookup"><span data-stu-id="27bb1-164">Verify name resolution is working</span></span>

<span data-ttu-id="27bb1-165">Po dokončení hello delegování, můžete ověřit, že funguje překlad adres pomocí nástroje, jako je například "nslookup" tooquery hello záznam SOA pro vaši zónu (ten je také automaticky vytvořený při vytváření zóny hello).</span><span class="sxs-lookup"><span data-stu-id="27bb1-165">After completing hello delegation, you can verify that name resolution is working by using a tool such as 'nslookup' tooquery hello SOA record for your zone (which is also automatically created when hello zone is created).</span></span>

<span data-ttu-id="27bb1-166">Nemají názvových serverů Azure DNS hello toospecify, pokud hello delegování byla nastavena správně, hello normální proces překladu DNS nalezne názvové servery hello automaticky.</span><span class="sxs-lookup"><span data-stu-id="27bb1-166">You do not have toospecify hello Azure DNS name servers, if hello delegation has been set up correctly, hello normal DNS resolution process finds hello name servers automatically.</span></span>

```
nslookup -type=SOA contoso.com
```

<span data-ttu-id="27bb1-167">Hello následuje odpověď příklad z hello předcházející příkaz:</span><span class="sxs-lookup"><span data-stu-id="27bb1-167">hello following is an example response from hello preceding command:</span></span>

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a><span data-ttu-id="27bb1-168">Delegování subdomén v Azure DNS</span><span class="sxs-lookup"><span data-stu-id="27bb1-168">Delegate sub-domains in Azure DNS</span></span>

<span data-ttu-id="27bb1-169">Pokud chcete tooset až samostatnou podřízenou zónu, můžete delegovat subdomény v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-169">If you want tooset up a separate child zone, you can delegate a sub-domain in Azure DNS.</span></span> <span data-ttu-id="27bb1-170">Například s nastavit a delegované "contoso.net" v Azure DNS Předpokládejme, že byste chtěli tooset až samostatnou podřízenou zónu, 'partners.contoso.net'.</span><span class="sxs-lookup"><span data-stu-id="27bb1-170">For example, having set up and delegated 'contoso.net' in Azure DNS, suppose you would like tooset up a separate child zone, 'partners.contoso.net'.</span></span>

1. <span data-ttu-id="27bb1-171">Partners.contoso.net' hello podřízenou zónu' vytvořte v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-171">Create hello child zone 'partners.contoso.net' in Azure DNS.</span></span>
2. <span data-ttu-id="27bb1-172">Vyhledejte záznamy autoritativních NS hello v hello podřízené zóny tooobtain hello názvové servery hostující podřízenou zónu hello v Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-172">Look up hello authoritative NS records in hello child zone tooobtain hello name servers hosting hello child zone in Azure DNS.</span></span>
3. <span data-ttu-id="27bb1-173">Delegát hello podřízenou zónu pomocí konfigurace záznamů NS v nadřazené zóně hello odkazující toohello podřízenou zónu.</span><span class="sxs-lookup"><span data-stu-id="27bb1-173">Delegate hello child zone by configuring NS records in hello parent zone pointing toohello child zone.</span></span>

### <a name="create-a-dns-zone"></a><span data-ttu-id="27bb1-174">Vytvoření zóny DNS</span><span class="sxs-lookup"><span data-stu-id="27bb1-174">Create a DNS zone</span></span>

1. <span data-ttu-id="27bb1-175">Přihlaste se toohello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="27bb1-175">Sign in toohello Azure portal</span></span>
1. <span data-ttu-id="27bb1-176">V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.</span><span class="sxs-lookup"><span data-stu-id="27bb1-176">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="27bb1-178">Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:</span><span class="sxs-lookup"><span data-stu-id="27bb1-178">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>

   | <span data-ttu-id="27bb1-179">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="27bb1-179">**Setting**</span></span> | <span data-ttu-id="27bb1-180">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="27bb1-180">**Value**</span></span> | <span data-ttu-id="27bb1-181">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="27bb1-181">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="27bb1-182">**Název**</span><span class="sxs-lookup"><span data-stu-id="27bb1-182">**Name**</span></span>|<span data-ttu-id="27bb1-183">partners.contoso.net</span><span class="sxs-lookup"><span data-stu-id="27bb1-183">partners.contoso.net</span></span>|<span data-ttu-id="27bb1-184">Název zóny DNS hello Hello</span><span class="sxs-lookup"><span data-stu-id="27bb1-184">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="27bb1-185">**Předplatné**</span><span class="sxs-lookup"><span data-stu-id="27bb1-185">**Subscription**</span></span>|<span data-ttu-id="27bb1-186">[Vaše předplatné]</span><span class="sxs-lookup"><span data-stu-id="27bb1-186">[Your subscription]</span></span>|<span data-ttu-id="27bb1-187">Vyberte bránu předplatné toocreate hello aplikace v.</span><span class="sxs-lookup"><span data-stu-id="27bb1-187">Select a subscription toocreate hello application gateway in.</span></span>|
   |<span data-ttu-id="27bb1-188">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="27bb1-188">**Resource group**</span></span>|<span data-ttu-id="27bb1-189">**Použít existující:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="27bb1-189">**Use Existing:** contosoRG</span></span>|<span data-ttu-id="27bb1-190">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="27bb1-190">Create a resource group.</span></span> <span data-ttu-id="27bb1-191">Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali.</span><span class="sxs-lookup"><span data-stu-id="27bb1-191">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="27bb1-192">Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.</span><span class="sxs-lookup"><span data-stu-id="27bb1-192">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="27bb1-193">**Umístění**</span><span class="sxs-lookup"><span data-stu-id="27bb1-193">**Location**</span></span>|<span data-ttu-id="27bb1-194">Západní USA</span><span class="sxs-lookup"><span data-stu-id="27bb1-194">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="27bb1-195">Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-195">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="27bb1-196">umístění zóny DNS Hello je vždy "globální" a není zobrazen.</span><span class="sxs-lookup"><span data-stu-id="27bb1-196">hello DNS zone location is always "global", and is not shown.</span></span>

### <a name="retrieve-name-servers"></a><span data-ttu-id="27bb1-197">Načtení názvových serverů</span><span class="sxs-lookup"><span data-stu-id="27bb1-197">Retrieve name servers</span></span>

1. <span data-ttu-id="27bb1-198">S zóny DNS hello vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="27bb1-198">With hello DNS zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="27bb1-199">Klikněte na tlačítko hello **partners.contoso.net** zónu DNS v hello **všechny prostředky** okno.</span><span class="sxs-lookup"><span data-stu-id="27bb1-199">Click hello **partners.contoso.net** DNS zone in hello **All resources** blade.</span></span> <span data-ttu-id="27bb1-200">Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **partners.contoso.net** v hello filtr podle názvu...</span><span class="sxs-lookup"><span data-stu-id="27bb1-200">If hello subscription you selected already has several resources in it, you can enter **partners.contoso.net** in hello Filter by name…</span></span> <span data-ttu-id="27bb1-201">pole tooeasily přístup hello DNS zóny.</span><span class="sxs-lookup"><span data-stu-id="27bb1-201">box tooeasily access hello DNS zone.</span></span>

1. <span data-ttu-id="27bb1-202">V okně zóny DNS hello načíst hello názvové servery.</span><span class="sxs-lookup"><span data-stu-id="27bb1-202">Retrieve hello name servers from hello DNS zone blade.</span></span> <span data-ttu-id="27bb1-203">V tomto příkladu hello zónu "contoso.net" přiřazené názvové servery ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS.NET', ' ns3-01.azure-dns.org', a ' ns4-01.azure-dns.info':</span><span class="sxs-lookup"><span data-stu-id="27bb1-203">In this example, hello zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="27bb1-205">Azure DNS automaticky vytvoří záznamy autoritativních NS ve vaší zóně obsahující hello přiřazené názvové servery.</span><span class="sxs-lookup"><span data-stu-id="27bb1-205">Azure DNS automatically creates authoritative NS records in your zone containing hello assigned name servers.</span></span>  <span data-ttu-id="27bb1-206">názvy toosee hello název serveru pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure, jednoduše musíte tooretrieve tyto záznamy.</span><span class="sxs-lookup"><span data-stu-id="27bb1-206">toosee hello name server names via Azure PowerShell or Azure CLI, you simply need tooretrieve these records.</span></span>

### <a name="create-name-server-record-in-parent-zone"></a><span data-ttu-id="27bb1-207">Vytvoření záznamu názvového serveru v nadřazené zóně</span><span class="sxs-lookup"><span data-stu-id="27bb1-207">Create name server record in parent zone</span></span>

1. <span data-ttu-id="27bb1-208">Přejděte toohello **contoso.net** zónu DNS v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="27bb1-208">Navigate toohello **contoso.net** DNS zone in hello Azure portal.</span></span>
1. <span data-ttu-id="27bb1-209">Klikněte na **+ Sada záznamů**.</span><span class="sxs-lookup"><span data-stu-id="27bb1-209">Click **+ Record set**</span></span>
1. <span data-ttu-id="27bb1-210">Na hello **přidat sadu záznamů** okno, zadejte následující hodnoty hello a pak klikněte na **OK**:</span><span class="sxs-lookup"><span data-stu-id="27bb1-210">On hello **Add record set** blade, enter hello following values, then click **OK**:</span></span>

   | <span data-ttu-id="27bb1-211">**Nastavení**</span><span class="sxs-lookup"><span data-stu-id="27bb1-211">**Setting**</span></span> | <span data-ttu-id="27bb1-212">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="27bb1-212">**Value**</span></span> | <span data-ttu-id="27bb1-213">**Podrobnosti**</span><span class="sxs-lookup"><span data-stu-id="27bb1-213">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="27bb1-214">**Název**</span><span class="sxs-lookup"><span data-stu-id="27bb1-214">**Name**</span></span>|<span data-ttu-id="27bb1-215">partners</span><span class="sxs-lookup"><span data-stu-id="27bb1-215">partners</span></span>|<span data-ttu-id="27bb1-216">Název zóny DNS podřízené hello Hello</span><span class="sxs-lookup"><span data-stu-id="27bb1-216">hello name of hello child DNS zone</span></span>|
   |<span data-ttu-id="27bb1-217">**Typ**</span><span class="sxs-lookup"><span data-stu-id="27bb1-217">**Type**</span></span>|<span data-ttu-id="27bb1-218">NS</span><span class="sxs-lookup"><span data-stu-id="27bb1-218">NS</span></span>|<span data-ttu-id="27bb1-219">Pro záznamy názvového serveru použijte NS.</span><span class="sxs-lookup"><span data-stu-id="27bb1-219">Use NS for name server records.</span></span>|
   |<span data-ttu-id="27bb1-220">**Hodnota TTL**</span><span class="sxs-lookup"><span data-stu-id="27bb1-220">**TTL**</span></span>|<span data-ttu-id="27bb1-221">1</span><span class="sxs-lookup"><span data-stu-id="27bb1-221">1</span></span>|<span data-ttu-id="27bb1-222">Čas toolive.</span><span class="sxs-lookup"><span data-stu-id="27bb1-222">Time toolive.</span></span>|
   |<span data-ttu-id="27bb1-223">**Jednotka hodnoty TTL**</span><span class="sxs-lookup"><span data-stu-id="27bb1-223">**TTL unit**</span></span>|<span data-ttu-id="27bb1-224">Hodiny</span><span class="sxs-lookup"><span data-stu-id="27bb1-224">Hours</span></span>|<span data-ttu-id="27bb1-225">Nastaví toohours toolive jednotka času</span><span class="sxs-lookup"><span data-stu-id="27bb1-225">sets time toolive unit toohours</span></span>|
   |<span data-ttu-id="27bb1-226">**NÁZVOVÝ SERVER**</span><span class="sxs-lookup"><span data-stu-id="27bb1-226">**NAME SERVER**</span></span>|<span data-ttu-id="27bb1-227">{názvové servery ze zóny partners.contoso.net}</span><span class="sxs-lookup"><span data-stu-id="27bb1-227">{name servers from partners.contoso.net zone}</span></span>|<span data-ttu-id="27bb1-228">Zadejte všechny 4 hello názvové servery z partners.contoso.net zóny.</span><span class="sxs-lookup"><span data-stu-id="27bb1-228">Enter all 4 of hello name servers from partners.contoso.net zone.</span></span> |

   ![Názvový server DNS](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a><span data-ttu-id="27bb1-230">Delegování subdomén v Azure DNS pomocí jiných nástrojů</span><span class="sxs-lookup"><span data-stu-id="27bb1-230">Delegating sub-domains in Azure DNS with other tools</span></span>

<span data-ttu-id="27bb1-231">Hello následující příklady popisují hello toodelegate subdomén v Azure DNS pomocí prostředí PowerShell a rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="27bb1-231">hello following examples provide hello steps toodelegate sub-domains in Azure DNS with PowerShell and CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="27bb1-232">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27bb1-232">PowerShell</span></span>

<span data-ttu-id="27bb1-233">Hello následující příklad PowerShell ukazuje, jak to funguje.</span><span class="sxs-lookup"><span data-stu-id="27bb1-233">hello following PowerShell example demonstrates how this works.</span></span> <span data-ttu-id="27bb1-234">Hello stejný postup lze provést prostřednictvím hello portál Azure nebo prostřednictvím hello rozhraní příkazového řádku Azure napříč platformami.</span><span class="sxs-lookup"><span data-stu-id="27bb1-234">hello same steps can be executed via hello Azure portal, or via hello cross-platform Azure CLI.</span></span>

```powershell
# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve hello authoritative NS records from hello child zone as shown in hello next example. This contains hello name servers assigned toohello child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create hello corresponding NS record set in hello parent zone toocomplete hello delegation. hello record set name in hello parent zone matches hello child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

<span data-ttu-id="27bb1-235">Použití `nslookup` tooverify, které všechno, co jsou nastaveny správně vyhledáním záznamu SOA podřízené zóny hello hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-235">Use `nslookup` tooverify that everything is set up correctly by looking up hello SOA record of hello child zone.</span></span>

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a><span data-ttu-id="27bb1-236">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="27bb1-236">Azure CLI</span></span>

```azurecli
#!/bin/bash

# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

<span data-ttu-id="27bb1-237">Načtení hello názvové servery pro hello `partners.contoso.net` zóny z výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-237">Retrieve hello name servers for hello `partners.contoso.net` zone from hello output.</span></span>

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="27bb1-238">Vytvořte hello sady záznamů a záznamy NS pro každý název serveru.</span><span class="sxs-lookup"><span data-stu-id="27bb1-238">Create hello record set and NS records for each name server.</span></span>

```azurecli
#!/bin/bash

# Create hello record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a><span data-ttu-id="27bb1-239">Odstranění všech prostředků</span><span class="sxs-lookup"><span data-stu-id="27bb1-239">Delete all resources</span></span>

<span data-ttu-id="27bb1-240">toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="27bb1-240">toodelete all resources created in this article, complete hello following steps:</span></span>

1. <span data-ttu-id="27bb1-241">V portálu Azure hello **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**.</span><span class="sxs-lookup"><span data-stu-id="27bb1-241">In hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="27bb1-242">Klikněte na tlačítko hello **contosorg** skupina prostředků v hello okno všechny prostředky.</span><span class="sxs-lookup"><span data-stu-id="27bb1-242">Click hello **contosorg** resource group in hello All resources blade.</span></span> <span data-ttu-id="27bb1-243">Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **contosorg** v hello **filtrovat podle názvu...**</span><span class="sxs-lookup"><span data-stu-id="27bb1-243">If hello subscription you selected already has several resources in it, you can enter **contosorg** in hello **Filter by name…**</span></span> <span data-ttu-id="27bb1-244">pole tooeasily přístup hello prostředků skupiny.</span><span class="sxs-lookup"><span data-stu-id="27bb1-244">box tooeasily access hello resource group.</span></span>
1. <span data-ttu-id="27bb1-245">V hello **contosorg** okně klikněte na tlačítko hello **odstranit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="27bb1-245">In hello **contosorg** blade, click hello **Delete** button.</span></span>
1. <span data-ttu-id="27bb1-246">Hello portál vyžaduje tootype hello název hello prostředků skupiny tooconfirm, které chcete toodelete ho.</span><span class="sxs-lookup"><span data-stu-id="27bb1-246">hello portal requires you tootype hello name of hello resource group tooconfirm that you want toodelete it.</span></span> <span data-ttu-id="27bb1-247">Typ *contosorg* hello název skupiny prostředků, klikněte **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="27bb1-247">Type *contosorg* for hello resource group name, then click **Delete**.</span></span> <span data-ttu-id="27bb1-248">Odstranění skupiny prostředků se odstraní všechny prostředky v rámci skupiny prostředků hello, takže vždy být zda tooconfirm hello obsah skupinu prostředků. před odstraněním.</span><span class="sxs-lookup"><span data-stu-id="27bb1-248">Deleting a resource group deletes all resources within hello resource group, so always be sure tooconfirm hello contents of a resource group before deleting it.</span></span> <span data-ttu-id="27bb1-249">Odstraní všechny prostředky obsažené v rámci skupiny prostředků hello Hello portálu, a pak odstraní samotná skupina prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="27bb1-249">hello portal deletes all resources contained within hello resource group, then deletes hello resource group itself.</span></span> <span data-ttu-id="27bb1-250">Tento proces trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="27bb1-250">This process takes several minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27bb1-251">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27bb1-251">Next steps</span></span>

[<span data-ttu-id="27bb1-252">Správa zón DNS</span><span class="sxs-lookup"><span data-stu-id="27bb1-252">Manage DNS zones</span></span>](dns-operations-dnszones.md)

[<span data-ttu-id="27bb1-253">Správa záznamů DNS</span><span class="sxs-lookup"><span data-stu-id="27bb1-253">Manage DNS records</span></span>](dns-operations-recordsets.md)
