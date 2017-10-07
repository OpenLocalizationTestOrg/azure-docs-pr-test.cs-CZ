---
title: "záznamy DNS aaaManage v Azure DNS pomocí hello 1.0 rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Správa sad záznamů DNS a záznamy v Azure DNS při hostování vaší domény ve službě Azure DNS. Všechny příkazy 1.0 rozhraní příkazového řádku pro operace na sady záznamů a záznamy."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 1f01450b0839f712cb1d96be318766bac581fea1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-in-azure-dns-using-hello-azure-cli-10"></a>Správa záznamů DNS v Azure DNS pomocí hello Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Tento článek ukazuje, jak hello toomanage záznamy DNS pro zónu DNS pomocí rozhraní příkazového řádku Azure (CLI), která je k dispozici pro Windows, Mac a Linux pro různé platformy. Můžete také spravovat svoje záznamy DNS pomocí [prostředí Azure PowerShell](dns-operations-recordsets.md) nebo hello [portál Azure](dns-operations-recordsets-portal.md).

## <a name="cli-versions-toocomplete-hello-task"></a>Úloha hello toocomplete verze rozhraní příkazového řádku

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

* [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -naše rozhraní příkazového řádku pro hello classic a resource správy nasazení modely.
* [Azure CLI 2.0](dns-operations-recordsets-cli.md) -naší nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.

Hello příklady v tomto článku předpokládá, že již máte [nainstalován hello 1.0 rozhraní příkazového řádku Azure, přihlášení a vytvořit zónu DNS](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Úvod

Před vytvořením záznamy DNS v Azure DNS, musíte nejdřív toounderstand jak Azure DNS jsou uspořádány záznamy DNS do sady záznamů DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

toocreate záznam DNS použijte hello `azure network dns record-set add-record` příkaz. Nápovědu získáte příkazem `azure network dns record-set add-record -h`.

Při vytváření záznamu, je třeba název skupiny prostředků hello toospecify, název zóny, název sady záznamů, typ záznamu hello a podrobnosti hello hello záznamu vytváří. Hello název sady záznamů musí být *relativní* název, což znamená, je nutné vyloučit hello název zóny.

Pokud sada záznamů hello již neexistuje, tento příkaz se automaticky vytvoří. Pokud již existuje sada záznamů hello, tento příkaz adda hello záznamu zadejte toohello existující sady záznamů.

Pokud se vytváří nová sada záznamů, použije se výchozí hodnota TTL (Time to Live) 3 600. Pokyny, jak toouse různých TTLs, najdete v části [vytvoření sady záznamů DNS](#create-a-dns-record-set).

Hello následující příklad vytvoří záznam názvem *www* v zóně hello *contoso.com* ve skupině prostředků hello *MyResourceGroup*. Hello IP adresu hello je záznam *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

toocreate záznam v hello vrcholu zóny hello (v tomto případě "contoso.com"), použijte hello název záznamu "@", včetně uvozovek hello:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>Vytvoření sady záznamů DNS

V hello výše příklady záznam DNS hello byla buď přidané tooan existující sady záznamů, nebo bylo vytvořeno sady záznamů hello *implicitně*. Můžete také vytvořit sadu záznamů hello *explicitně* před přidávání záznamů tooit. Azure DNS podporuje 'prázdné, sady záznamů, které může fungovat jako zástupný symbol tooreserve název DNS před vytvořením záznamy DNS. Prázdný sady záznamů jsou viditelné v rovině řízení Azure DNS, ale nejsou zobrazeny na názvových serverů Azure DNS hello hello.

Sady záznamů jsou vytvořené pomocí hello `azure network dns record-set create` příkaz. Nápovědu získáte příkazem `azure network dns record-set create -h`.

Vytvoření záznamu hello explicitně nastaven umožňuje vlastnosti sady záznamů toospecify například hello [Time To Live (TTL)](dns-zones-records.md#time-to-live) a metadata. [Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota.

Hello následující příklad vytvoří záznam prázdný nastavit hodnotu TTL 60 sekund pomocí hello `--ttl` parametr (krátký tvar `-l`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

Hello následující příklad vytvoří záznam s dvě položky metadat "oddělení = finance" a "prostředí = produkční", pomocí hello `--metadata` parametr (krátkodobých formuláře `-m`):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Vytvoření prázdná sada záznamů záznamy lze přidat pomocí `azure network dns record-set add-record` jak je popsáno v [vytvořit záznam DNS](#create-a-dns-record).

## <a name="create-records-of-other-types"></a>Vytvořte záznamy jiné typy

S vidět podrobně jak toocreate "A" záznamy, hello následující příklady zobrazují jak toocreate záznam jiné typy záznamů podporované Azure DNS.

Parametry Hello použít toospecify hello záznam, který dat se liší v závislosti na typu hello hello záznamu. Například pro záznam typu "A", jste zadali hello IPv4 adresu s parametrem hello `-a <IPv4 address>`. Hello parametry pro každý typ záznamu může být uvedený pomocí `azure network dns record-set add-record -h`.

V každém případě ukážeme, jak toocreate jeden záznam. záznam Hello je přidaný toohello existující sady záznamů nebo vytvořit sadu záznamů implicitně. Další informace o vytvoření sady záznamů a definování záznam nastavte parametr explicitně najdete v tématu [vytvoření sady záznamů DNS](#create-a-dns-record-set).

Jsme neudělujte toocreate příklad sady záznamů SOA vzhledem k tomu, že se vytvoří SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně. Ale [hello SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record"></a>Vytvoření záznamů AAAA

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a>Vytvořte záznam CNAME

> [!NOTE]
> standardech DNS Hello nepovoluje záznamy CNAME v hello vrcholu zóny (`-Name "@"`), ani se povolit sad záznamů obsahující více než jeden záznam.
> 
> Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Vytvoření záznamů MX

V tomto příkladu používáme hello název sady záznamů "@" hello toocreate záznam MX ve vrcholu zóny hello (v tomto případě "contoso.com").

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Vytvoření záznamů NS

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>Vytvořit záznam PTR

V takovém případě se Moje-arpa-server zone.com' představuje hello zóny ARPA představující rozsahu IP adres. Každý záznam PTR nastavení v této zóně odpovídá tooan IP adresu do tohoto rozsahu IP adres.  název záznamu Hello "10" je poslední oktet hello hello IP adresy v tomto rozsahu IP reprezentována tento záznam.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Vytvoření záznamů SRV

Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte hello  *\_služby* a  *\_protokol* v hello název sady záznamu. Neexistuje žádné tooinclude nutné "@" v hello název sady záznamu při vytváření záznamu SRV nastavit na vrcholu zóny hello.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>Vytvořit záznam TXT

Hello následující příklad ukazuje, jak záznam toocreate TXT. Další informace o hello maximální délka řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Získání sady záznamů

tooretrieve existující sady záznamů, použijte `azure network dns record-set show`. Nápovědu získáte příkazem `azure network dns record-set show -h`.

Při vytváření záznamu nebo sady záznamů, sady záznamů hello název musí mít *relativní* název, což znamená, je nutné vyloučit hello název zóny. Musíte taky typ záznamu hello toospecify, hello zóny hello záznam obsahující nastavení a hello skupinu prostředků obsahující hello zóny.

Hello následující příklad načte záznam hello *www* a typ ze zóny *contoso.com* ve skupině prostředků *MyResourceGroup*:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Seznam sad záznamů

Můžete vytvořit seznam všech záznamů v zóně DNS pomocí hello `azure network dns record-set list` příkaz. Nápovědu získáte příkazem `azure network dns record-set list -h`.

Tento příklad vrátí sady všech záznamů v zóně hello *contoso.com*, ve skupině prostředků *MyResourceGroup*bez ohledu na název a typ záznamu:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

Tento příklad vrátí všechny sady záznamů, které odpovídají hello daného typu záznamu (v tomto případě "A" záznamů):

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-tooan-existing-record-set"></a>Přidání záznamů tooan existující sady záznamů

Můžete použít `azure network dns record-set add-record` sada záznamů v novém záznamu toocreate i tooadd stávající záznam záznamů tooan nastavit.

Další informace najdete v tématu [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.

## <a name="remove-a-record-from-an-existing-record-set"></a>Odeberte záznam z existující sady záznamů.

tooremove DNS záznam z existující sady záznamů, použijte `azure network dns record-set delete-record`. Nápovědu získáte příkazem `azure network dns record-set delete-record -h`.

Tento příkaz odstraní záznam DNS ze sady záznamů. Pokud se odstraní poslední záznam hello v sadě záznamů, samotné sady záznamů hello je **není** odstranit. Místo toho je ponecháno prázdná sada záznamů. Místo toho sady záznamů hello toodelete najdete v části [odstranit sadu záznamů](#delete-a-record-set).

Je třeba toospecify hello záznamů toobe odstranit a hello zónu, je nutné ji odstranit, pomocí hello stejnými parametry, jako při vytváření záznamů pomocí `azure network dns record-set add-record`. Tyto parametry jsou popsané v [vytvořit záznam DNS](#create-a-dns-record) a [vytvořit záznamy jiné typy](#create-records-of-other-types) výše.

Tento příkaz zobrazí výzvu k potvrzení. Tuto výzvu lze potlačit pomocí hello `--quiet` přepínače (krátký tvar `-q`).

Následující příklad odstranění Hello hello záznam s hodnotou '1.2.3.4"ze záznamu hello nastavení s názvem *www* v zóně hello *contoso.com*, ve skupině prostředků hello *MyResourceGroup*. výzva k potvrzení Hello potlačeno.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Upravit existující sady záznamů

Každá sada záznamů obsahuje [time to live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata)a záznamy DNS. Hello následující oddíly popisují, jak toomodify těchto vlastností.

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>toomodify záznam A, AAAA, MX, NS, PTR, SRV a TXT

toomodify existujícího záznamu typu A, AAAA, MX, NS, PTR, SRV nebo TXT, měli byste nejprve přidat nový záznam a pak odstraňte hello stávajícího záznamu. Pro podrobné pokyny, jak toodelete a přidat záznamy, najdete v části hello dřívější části tohoto článku.

Hello následující příklad ukazuje, jak toomodify záznam "A", z IP adres 1.2.3.4 tooIP adres 5.6.7.8:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="toomodify-a-cname-record"></a>toomodify záznam CNAME

použít toomodify záznam CNAME `azure network dns record-set add-record` tooadd hello nová hodnota záznamu. Na rozdíl od jiných typů záznamů sadu záznamů CNAME obsahovat jenom jeden záznam. Je proto hello stávajícího záznamu *nahradit* při nový záznam hello je přidána a nemusí toobe odstranit samostatně.  Vám bude výzvami tooaccept tento nahrazení.

Hello příklad upravuje sadu záznamů CNAME hello *www* v zóně hello *contoso.com*, ve skupině prostředků *MyResourceGroup*, toopoint příliš 'www.fabrikam.net"namísto jeho stávající hodnotu:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a>toomodify záznamu SOA

Použití `azure network dns record-set set-soa-record` toomodify hello SOA pro danou zónu DNS. Nápovědu získáte příkazem `azure network dns record-set set-soa-record -h`.

Hello následující příklad ukazuje, jak vlastnost e-mailu, tooset hello"hello SOA záznam pro zóny hello *contoso.com* ve skupině prostředků hello *MyResourceGroup*:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="toomodify-ns-records-at-hello-zone-apex"></a>toomodify NS záznamy na vrcholu zóny hello

Nastavte na vrcholu zóny hello záznam NS Hello se vytvoří automaticky s každou zónou DNS. Obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny.

Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény. Můžete také upravit hello TTL a metadat pro tuto sadu záznamů. Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS.

Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello. Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) můžete změnit bez omezení.

Hello následující příklad ukazuje, jak tooadd záznam NS toohello další název serveru nastavit na vrcholu zóny hello:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a>nastavit toomodify hello TTL existujícího záznamu

nastavení toomodify hello TTL existujícího záznamu, použijte `azure network dns record-set set`. Nápovědu získáte příkazem `azure network dns record-set set -h`.

Hello následující příklad ukazuje, jak toomodify sadu záznamů TTL, v tomto případě too60 sekund:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a>metadata hello toomodify existující sady záznamů

[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota. nastavení toomodify hello metadata existujícího záznamu, použijte `azure network dns record-set set`. Nápovědu získáte příkazem `azure network dns record-set set -h`.

Hello následující příklad ukazuje, jak toomodify sada záznamů s dvě položky metadat "oddělení = finance" a "prostředí = produkční", pomocí hello `--metadata` parametr (krátkodobých formuláře `-m`). Všimněte si, že všechny existující metadata jsou *nahradit* podle zadané hodnoty hello.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>Odstranit sadu záznamů

Sady záznamů lze odstranit pomocí hello `azure network dns record-set delete` příkaz. Nápovědu získáte příkazem `azure network dns record-set delete -h`. Odstranění sady záznamů také odstraní všechny záznamy v sadě záznamů hello.

> [!NOTE]
> Nelze odstranit hello SOA a NS sady záznamů na vrcholu zóny hello (`-Name "@"`).  Tyto soubory jsou vytvořeny automaticky při hello zóny byl vytvořen a jsou automaticky odstraněna při odstranění zóny hello.

Hello následující příklad odstraní sadu s názvem záznamů hello *www* a typ ze zóny hello *contoso.com* ve skupině prostředků *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

Operace odstranění hello výzvami tooconfirm jste. toosuppress tento řádek, použijte hello `--quiet` přepínače (krátký tvar `-q`).

## <a name="next-steps"></a>Další kroky

Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).
<br>
Zjistěte, jak příliš[chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.
