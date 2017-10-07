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
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a>Hostování zpětné zóny DNS vyhledávání v Azure DNS

Tento článek vysvětluje, jak toohost hello DNS zóny zpětného vyhledávání pro váš přiřazené rozsahy IP v Azure DNS. rozsahy IP Hello reprezentována zóny zpětného vyhledávání hello musí být přiřazen tooyour organizace, obvykle podle vašeho poskytovatele internetových služeb.

tooconfigure zpětně DNS pro Azure vlastní IP adresu přiřazenou tooyour služba Azure, najdete v části [konfigurovat hello zpětného vyhledávání pro hello IP adresy přidělené tooyour služba Azure](dns-reverse-dns-for-azure-services.md).

Před přečtení tohoto článku, měli byste se seznámit s tím [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md).

Tento článek vás provede kroky toocreate hello první zóny zpětného vyhledávání DNS a záznam pomocí hello portál Azure, Azure PowerShell, Azure CLI 1.0 nebo 2.0 rozhraní příkazového řádku Azure.

## <a name="create-a-reverse-lookup-dns-zone"></a>Vytvořit zónu zpětného vyhledávání DNS

1. Přihlaste se toohello [portálu Azure](https://portal.azure.com)
1. V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový** > **sítě** > a pak klikněte na **zónu DNS** tooopen hello **zóny DNS vytvořte**okno.

   ![Zóna DNS](./media/dns-reverse-dns-hosting/figure1.png)

1. Na hello **zóny DNS vytvořte** okně název zóny DNS. Hello název zóny hello je jinak vytvořených pro předpony IPv4 a IPv6. Použijte buď hello pokyny pro [IPV4](#ipv4) nebo [IPv6](#ipv6) tooname vaši zónu. Po dokončení klikněte na tlačítko **vytvořit** toocreate hello zóny.

### <a name="ipv4"></a>IPv4

Název Hello zóny zpětného vyhledávání IPv4 je založen na rozsah hello IP adres, který představuje. Musí být ve formátu hello: `<IPv4 network prefix in reverse order>.in-addr.arpa`. Příklady najdete v tématu [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md#ipv4).

> [!NOTE]
> Při vytváření classless zóny zpětného vyhledávání DNS v Azure DNS, je nutné použít pomlčka (`-`) namísto lomítkem (/') v názvu zóny hello.
>
> Například pro hello 192.0.2.128/26 rozsah IP, musíte použít `128-26.2.0.192.in-addr.arpa` jako název zóny hello místo `128/26.2.0.192.in-addr.arpa`.
>
> Důvodem je, že když jsou oba podporované ve standardech DNS hello, zón služby DNS názvy obsahující hello lomítkem (`/`) znak nejsou podporovány v Azure DNS.

Hello následující příklad ukazuje, jak toocreate třídy C reverse zónu DNS s názvem `2.0.192.in-addr.arpa` v Azure DNS prostřednictvím hello portálu Azure:

 ![Vytvoření zóny DNS](./media/dns-reverse-dns-hosting/figure2.png)

Definuje hello umístění pro skupinu prostředků hello Hello umístění skupiny prostředků a nemá žádný vliv na zónu DNS hello. umístění zóny DNS Hello je vždy "globální a není zobrazen.

Hello následující příklady ukazují, jak toocomplete tato úloha pomocí Azure PowerShell a rozhraní příkazového řádku Azure hello:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Název Hello zóny zpětného vyhledávání IPv6 by měl být v hello následující formulář: `<IPv6 network prefix in reverse order>.ip6.arpa`.  Příklady najdete v tématu [přehled zpětné DNS a podpory v Azure](dns-reverse-dns-overview.md#ipv6).


Hello následující příklad ukazuje, jak toocreate zóny vyhledávání DNS zpětného IPv6 s názvem `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` v Azure DNS prostřednictvím hello portálu Azure:

 ![Vytvoření zóny DNS](./media/dns-reverse-dns-hosting/figure3.png)

Definuje hello umístění pro skupinu prostředků hello Hello umístění skupiny prostředků a nemá žádný vliv na zónu DNS hello. umístění zóny DNS Hello je vždy "globální a není zobrazen.

Hello následující příklady ukazují, jak toocomplete tato úloha pomocí Azure PowerShell a rozhraní příkazového řádku Azure hello:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a>Delegování zóny zpětného vyhledávání DNS

Vytvoření zóny zpětného vyhledávání DNS je nutné zajistit, že je zóny hello delegovaná z nadřazené zóně hello. Delegování DNS umožňuje hello DNS název řešení proces toofind hello názvové servery hostující vaše zóny zpětného vyhledávání DNS. To umožňuje tyto název servery tooanswer zpětné dotazy DNS pro hello IP adresy ve vaší rozsah adres.

Pro zóny dopředného vyhledávání, je popsán proces hello delegování zóny DNS v [delegovat tooAzure vaší domény DNS](dns-delegate-domain-azure-dns.md). Delegování zón zpětného vyhledávání funguje hello stejným způsobem. Hello jediným rozdílem je, že je potřeba tooconfigure hello názvové servery s hello poskytovatele internetových služeb, který poskytuje rozsahu IP adres, nikoli vaším registrátorem názvu domény.

## <a name="create-a-dns-ptr-record"></a>Vytvořit záznam DNS PTR

### <a name="ipv4"></a>IPv4

Hello následující příklad vás provede procesem hello vytvoření záznam PTR v zpětné zóny DNS v Azure DNS. Pro jiné typy záznamů a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí portálu Azure hello](dns-operations-recordsets-portal.md).

1.  Hello horní části hello **zónu DNS** vyberte **+ sady záznamů** tooopen hello **přidat sadu záznamů** okno.

 ![Zóna DNS](./media/dns-reverse-dns-hosting/figure4.png)

1. Na hello **přidat sadu záznamů** okno. 
1. Vyberte **PTR** ze záznamu hello "**typ**" nabídky.  
1. Hello název sady záznamů hello pro záznam PTR musí toobe hello zbytek hello adresa IPv4 v obráceném pořadí. V tomto příkladu hello první tři oktety již naplní se jako součást název zóny hello (.2.0.192). Proto je pouze poslední oktet hello zadat v poli Název hello. Například mohla mít název vaší sady záznamů "**15**" pro prostředek, jehož IP adresa je 192.0.2.15.  
1. V hello "**název domény**" Zadejte hello plně kvalifikovaný název domény (FQDN) hello prostředku pomocí hello IP.
1. Vyberte **OK** dole hello záznam DNS hello okno toocreate hello.

 ![Přidat sadu záznamů](./media/dns-reverse-dns-hosting/figure5.png)

Hello následuje několik příkladů, jak toocomplete tato úloha pomocí prostředí PowerShell a hello AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a>AzureCLI 1.0

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a>IPv6

Hello následující ukázka vás provede procesem vytvoření nového záznamu, PTR, hello. Pro jiné typy záznamů a toomodify existující záznamy, najdete v části [záznamy DNS spravovat a sad záznamů pomocí portálu Azure hello](dns-operations-recordsets-portal.md).

1. Hello horní části hello **okno zóny DNS**, vyberte **+ sady záznamů** tooopen hello **přidat sadu záznamů** okno.

  ![okno zóny DNS](./media/dns-reverse-dns-hosting/figure6.png)

2. Na hello **přidat sadu záznamů** okno. 
3. Vyberte **PTR** ze záznamu hello "**typ**" nabídky.  
4. Hello název sady záznamů hello pro záznam PTR musí toobe hello zbytek hello adresu IPv6 v obráceném pořadí. Nesmí obsahovat nula komprese. V tomto příkladu hello první 64bitová verze hello IPv6 jsou už vyplněné jako součást název zóny hello (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa). Proto se zadávají pouze hello posledních 64 bitů v poli Název hello. Hello poslední 64 bitů hello IP adresy se zadávají v obráceném pořadí pomocí dobou jako oddělovače hello mezi každou hexadecimální číslo. Například mohla mít název vaší sady záznamů "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" pro prostředek, jehož IP adresa je 2001:0db8:abdc:0000:f524:10bc:1af9:405e.  
5. V hello "**název domény**" Zadejte hello plně kvalifikovaný název domény (FQDN) hello prostředku pomocí hello IP.
6. Vyberte **OK** dole hello záznam DNS hello okno toocreate hello.

![Přidat okno sady záznamů](./media/dns-reverse-dns-hosting/figure7.png)

Hello následuje několik příkladů, jak toocomplete tato úloha pomocí prostředí PowerShell a hello AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a>AzureCLI 1.0

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a>AzureCLI 2.0

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a>Zobrazení záznamů

tooview hello záznamy, které jste vytvořili, přejděte tooyour zónu DNS v hello portálu Azure. V hello nižší součástí hello **zónu DNS** okně uvidíte hello záznamy pro zónu DNS hello. Měli byste vidět hello výchozí NS a SOA záznamů, které jsou vytvořené v každé zóny, a všechny nové záznamy, které jste vytvořili.

### <a name="ipv4"></a>IPv4

Okno zóny DNS, zobrazují se záznamy IPv4 PTR:

![okno zóny DNS](./media/dns-reverse-dns-hosting/figure8.png)

Hello následující příklady ukazují, jak tooview hello PTR záznamy pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure hello:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a>IPv6

Okno zóny DNS, zobrazují se záznamy IPv6 PTR:

![okno zóny DNS](./media/dns-reverse-dns-hosting/figure9.png)

Hello Následují příklady jak tooview hello záznamy pomocí prostředí PowerShell a hello AzureCLI:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a>Nejčastější dotazy

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Může hostovat zóny zpětného vyhledávání DNS pro moje bloky IP přiřazené poskytovatele internetových služeb ve službě Azure DNS?

Ano. Hostování hello zóny zpětného vyhledávání (ARPA) pro vlastní rozsahy IP v Azure DNS je plně podporována.

Vytvořte zóny zpětného vyhledávání hello v Azure DNS jak je vysvětleno v tomto článku a pracovat s vašeho poskytovatele internetových služeb příliš[delegáta hello zóny](dns-domain-delegation.md).  Pak můžete spravovat hello záznamů PTR pro každý zpětného vyhledávání v hello stejným způsobem jako ostatní typy záznamů.

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a>Kolik nemá hostování Moje zpětné zóny náklady vyhledávání DNS?

Hostování hello zpětné zóny vyhledávání DNS pro blok vašeho poskytovatele internetových služeb přiřadit IP v Azure DNS je účtován na [standardních sazeb Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Může hostovat zpětné zóny vyhledávání DNS pro adresu IPv4 i IPv6 adresu v Azure DNS?

Ano. Tento článek vysvětluje, jak toocreate oba protokoly IPv4 a IPv6 DNS zóny zpětného vyhledávání v Azure DNS.

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a>Můžete importovat existující zóně zpětného vyhledávání DNS?

Ano. Můžete vytvořit hello rozhraní příkazového řádku Azure tooimport existující zóny DNS do Azure DNS. Tento postup funguje pro zóny dopředného vyhledávání a zóny zpětného vyhledávání.

Další informace najdete v tématu [Import a export souboru zóny DNS pomocí rozhraní příkazového řádku Azure hello](dns-import-export.md).

## <a name="next-steps"></a>Další kroky

Další informace o zpětné DNS najdete v tématu [zpětného vyhledávání DNS na webu Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Zjistěte, jak příliš[zpětné záznamy DNS pro služeb Azure spravovat](dns-reverse-dns-for-azure-services.md).
