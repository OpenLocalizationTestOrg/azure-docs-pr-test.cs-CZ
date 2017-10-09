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
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>Správa záznamů DNS a sady záznamů v Azure DNS pomocí Azure PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Tento článek ukazuje, jak toomanage DNS záznamy pro zóny DNS pomocí prostředí Azure PowerShell. Záznamy DNS se taky dají spravovat pomocí hello napříč platformami [rozhraní příkazového řádku Azure](dns-operations-recordsets-cli.md) nebo hello [portál Azure](dns-operations-recordsets-portal.md).

Hello příklady v tomto článku předpokládá, že již máte [nainstalovaný Azure PowerShell, přihlášení a vytvořit zónu DNS](dns-operations-dnszones.md).

## <a name="introduction"></a>Úvod

Před vytvořením záznamy DNS v Azure DNS, musíte nejdřív toounderstand jak Azure DNS jsou uspořádány záznamy DNS do sady záznamů DNS.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Další informace o záznamech DNS v DNS Azure najdete v tématu [Zóny a záznamy DNS](dns-zones-records.md).


## <a name="create-a-new-dns-record"></a>Vytvořit nový záznam DNS

Pokud má nový záznam hello stejný název a zadejte jako stávající záznam, je třeba příliš[ho přidat existující sady záznamů toohello](#add-a-record-to-an-existing-record-set). Pokud nový záznam má jiný název a typ tooall, existující záznamy, je třeba toocreate nové sady záznamů. 

### <a name="create-a-records-in-a-new-record-set"></a>Vytvořit "A" záznamy v nové sady záznamů

Vytvoření sady záznamů pomocí hello `New-AzureRmDnsRecordSet` rutiny. Při vytváření sady záznamů, je třeba název sady záznamů hello toospecify, hello zóny, době hello toolive (TTL), typ záznamu hello a hello záznamy toobe vytvoření.

Hello parametry pro přidání sady záznamů tooa záznamů se liší v závislosti na typu hello hello sady záznamů. Například pokud používáte sadu záznamů typu "A", budete potřebovat toospecify hello IP adresu pomocí parametru hello `-IPv4Address`. Další parametry jsou používány pro jiné typy záznamů. V tématu [Příklady dalších typů záznamu](#additional-record-type-examples) podrobnosti.

Hello následující příklad vytvoří záznamů s relativním názvem "www" v zóně DNS "contoso.com" hello hello. Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com". Typ záznamu Hello je "A" a je hello TTL 3 600 sekund. Hello sada záznamů obsahuje jediný záznam, s IP adresou: 1.2.3.4'.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

toocreate sada záznamů v hello vrcholu zóny (v tomto případě "contoso.com"), použijte název sady záznamů hello ' @' (bez uvozovek):

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Pokud potřebujete toocreate sada záznamů obsahující více než jeden záznam, místní pole a přidat záznamy hello vytvořte nejdříve předat hello pole příliš`New-AzureRmDnsRecordSet` následujícím způsobem:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota. Hello následující příklad ukazuje, jak toocreate sada záznamů s dvě položky metadat se oddělení = finanční ' a ' prostředí = produkční '.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS podporuje také 'prázdné, sady záznamů, které může fungovat jako zástupný symbol tooreserve název DNS před vytvořením záznamy DNS. Prázdný sady záznamů jsou viditelné v rovině řízení hello Azure DNS, ale zobrazí na názvových serverů Azure DNS hello. Hello následující ukázka vytvoří prázdná sada záznamů:

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Vytvořte záznamy jiné typy

S vidět podrobně jak toocreate "A" záznamy, hello následující příklady zobrazují jak záznam toocreate záznamy jiné typy podporované systémem Azure DNS.

V každém případě ukážeme, jak nastavit toocreate záznam obsahující jeden záznam. Hello starší příklady záznamy 'A' může být přizpůsobena toocreate sady záznamů jiných typů obsahující více záznamů s metadaty, nebo sady záznamů toocreate prázdný.

Jsme neudělujte toocreate příklad sady záznamů SOA vzhledem k tomu, že se vytvoří SOAs a odstranit s každou zónou DNS a nelze vytvořit nebo odstranit samostatně. Ale [hello SOA lze upravit, jak je znázorněno v příkladu novější](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Vytvoření sady záznamů AAAA s jedním záznamem

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Vytvoření sady záznamů CNAME s jedním záznamem

> [!NOTE]
> standardech DNS Hello nepovoluje záznamy CNAME v hello vrcholu zóny (`-Name '@'`), ani se povolit sad záznamů obsahující více než jeden záznam.
> 
> Další informace najdete v tématu [záznamy CNAME](dns-zones-records.md#cname-records).


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Vytvoření sady záznamů MX s jedním záznamem

V tomto příkladu používáme hello název sady záznamů "@" záznam toocreate MX ve vrcholu zóny hello (v tomto případě "contoso.com").


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Vytvoření sady záznamů NS s jedním záznamem

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Vytvoření sady záznamů PTR s jedním záznamem

V takovém případě se Moje-arpa-server zone.com' představuje hello zóny zpětného vyhledávání ARPA představující rozsahu IP adres. Každý záznam PTR nastavení v této zóně odpovídá tooan IP adresu do tohoto rozsahu IP adres. název záznamu Hello "10" je poslední oktet hello hello IP adresy v tomto rozsahu IP reprezentována tento záznam.

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Vytvoření sady záznamů SRV s jedním záznamem

Při vytváření [sady záznamů SRV](dns-zones-records.md#srv-records), zadejte hello  *\_služby* a  *\_protokol* v hello název sady záznamu. Neexistuje žádné tooinclude nutné ' @' v hello název sady záznamu při vytváření záznamu SRV nastavit na vrcholu zóny hello.

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Vytvořit záznam TXT s jedním záznamem

Hello následující příklad ukazuje, jak záznam toocreate TXT. Další informace o hello maximální délka řetězce v záznamů TXT podporovány, naleznete v části [záznamů TXT](dns-zones-records.md#txt-records).

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Získání sady záznamů

tooretrieve existující sady záznamů, použijte `Get-AzureRmDnsRecordSet`. Tato rutina vrátí místní objekt, který reprezentuje hello sady záznamů v Azure DNS.

Stejně jako u `New-AzureRmDnsRecordSet`, musí být zadaný název sady záznamů hello *relativní* název, což znamená, je nutné vyloučit hello název zóny. Musíte také typ záznamu hello toospecify a časového pásma hello obsahující sadu záznamů hello.

Hello následující příklad ukazuje, jak tooretrieve sada záznamů. V tomto příkladu je zadána hello zóna pomocí hello `-ZoneName` a `-ResourceGroupName` parametry.

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Alternativně můžete také určit hello zóny pomocí objekt zóny, předaným pomocí hello `-Zone` parametr.

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Seznam sad záznamů

Můžete také použít `Get-AzureRmDnsZone` sady toolist záznamů v zóně, díky vynechání hello `-Name` nebo `-RecordType` parametry.

Hello následující příklad vrací sady všech záznamů v zóně hello:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Hello následující příklad ukazuje, jak všechny sady daného typu záznamů můžete načíst tak, že zadáte typ záznamu hello při vynechání hello záznam sady název:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

tooretrieve sady všech záznamů se zadaným názvem mezi typy záznamů, budete potřebovat tooretrieve všechny sady záznamů a pak výsledky hello filtru:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

Ve všech hello výše příklady hello zóna může být zadán buď pomocí hello `-ZoneName` a `-ResourceGroupName`parametry (jak je znázorněno), nebo zadáním objekt zóny:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-tooan-existing-record-set"></a>Přidání záznamů tooan existující sady záznamů

tooadd stávající záznam záznamů tooan nastavit, postupujte podle hello následující tři kroky:

1. Získat hello existující sady záznamů

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Přidejte hello nového záznamu toohello místní sady záznamů. Toto je offline operace.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Potvrďte hello změnu zpět toohello služba Azure DNS. 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

Pomocí `Set-AzureRmDnsRecordSet` *nahrazuje* hello existující sady záznamů v Azure DNS (a všechny záznamy, které obsahuje) s sady záznamů hello zadán. [Značka Etag kontroly](dns-zones-records.md#etags) používají tooensure souběžných změny se nepřepíšou. Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.

Tato posloupnost operací může být také *přesměruje*, což znamená předat objekt sady záznamů hello pomocí kanálu hello spíše než předání jako parametr:

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Příklady Hello výše ukazují, jak nastavit tooadd "A" záznamu tooan existujícího záznamu typu "A". Podobně jako posloupnost operací je toorecord sad záznamů použité tooadd jiných typů, nahraďte hello `-Ipv4Address` parametr `Add-AzureRmDnsRecordConfig` s jiným typem záznamu konkrétní tooeach parametry. Hello parametry pro každý typ záznamu jsou hello stejné jako u hello `New-AzureRmDnsRecordConfig` rutiny, jak je znázorněno v [Příklady dalších typů záznamu](#additional-record-type-examples) výše.

Sady záznamů typu 'CNAME' nebo 'SOA, nemůže obsahovat více než jeden záznam. Toto omezení vyplývá z norem DNS hello. Není omezení Azure DNS.

## <a name="remove-a-record-from-an-existing-record-set"></a>Odebrat záznam z existující sady záznamů

Hello tooremove procesu vytvoří záznam na sadu záznamů je podobný procesu tooadd toohello záznamů tooan existující sady záznamů:

1. Získat hello existující sady záznamů

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Odeberte záznam hello z objektu hello místní sady záznamů. Toto je offline operace. Hello záznam, který má být odebrán musí být přesně shodovat s existujícím záznamem v rámci všech parametrů.

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Potvrďte hello změnu zpět toohello služba Azure DNS. Volitelné použití hello `-Overwrite` přepínač toosuppress [kontroluje Etag](dns-zones-records.md#etags) souběžných změny.

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

Pomocí hello výše pořadí tooremove hello poslední záznam ze záznamů sady neodstraní hello sady záznamů, místo zůstanou prázdná sada záznamů. tooremove sada záznamů zcela, najdete v části [odstranit sadu záznamů](#delete-a-record-set).

Podobně tooadding záznamy tooa sady záznamů, hello posloupnost operací tooremove, které lze předat také sada záznamů:

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Různé typy záznamů jsou podporovány předáním příslušné parametry specifické pro typ hello příliš`Remove-AzureRmDnsRecordSet`. Hello parametry pro každý typ záznamu jsou hello stejné jako u hello `New-AzureRmDnsRecordConfig` rutiny, jak je znázorněno v [Příklady dalších typů záznamu](#additional-record-type-examples) výše.


## <a name="modify-an-existing-record-set"></a>Upravit existující sady záznamů

kroky Hello úpravy existující sady záznamů jsou podobné toohello kroky, jak při přidávání nebo odebírání záznamů z sada záznamů:

1. Načtení záznamu stávajícího hello nastavit pomocí `Get-AzureRmDnsRecordSet`.
2. Upravte objekt hello místní sady záznamů podle:
    * Přidání nebo odebrání záznamů
    * Změna parametrů hello existující záznamy
    * Změna záznamu hello nastavte metadata a čas toolive (TTL)
3. Potvrdit změny pomocí hello `Set-AzureRmDnsRecordSet` rutiny. To *nahrazuje* hello existující sady záznamů v Azure DNS s sady záznamů hello zadán.

Při použití `Set-AzureRmDnsRecordSet`, [kontroluje Etag](dns-zones-records.md#etags) používají tooensure souběžných změny se nepřepíšou. Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.

### <a name="tooupdate-a-record-in-an-existing-record-set"></a>nastavit tooupdate záznam do stávajícího záznamu

V tomto příkladu Změníme hello IP adresu existující "A" záznamu:

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-an-soa-record"></a>toomodify záznamu SOA

Nelze přidat nebo odebrat záznamy z hello automaticky vytvořil záznam SOA nastavit na vrcholu zóny hello (`-Name "@"`, včetně uvozovek nahoře). Ale můžete upravit některý z parametrů hello v rámci hello záznam SOA (s výjimkou "hostitel") a záznam hello nastavit hodnotu TTL.

Následující příklad ukazuje, jak Hello toochange hello *e-mailu* vlastnost hello záznam SOA:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a>toomodify NS záznamy na vrcholu zóny hello

Nastavte na vrcholu zóny hello záznam NS Hello se vytvoří automaticky s každou zónou DNS. Obsahuje názvy hello hello Azure DNS název servery přiřazené toohello zóny.

Můžete přidat další název servery toothis NS sady záznamů, toosupport společně s více než jednoho poskytovatele DNS hostování domény. Můžete také upravit hello TTL a metadat pro tuto sadu záznamů. Však nelze odebrat ani změnit hello předem vyplněná názvových serverů Azure DNS.

Všimněte si, že platí pouze toohello NS sady záznamů na vrcholu zóny hello. Další NS sady záznamů ve vaší zóně (jako podřízené zóny použité toodelegate) můžete změnit bez omezení.

Hello následující příklad ukazuje, jak tooadd záznam NS toohello další název serveru nastavit na vrcholu zóny hello:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-record-set-metadata"></a>sady záznamů toomodify metadat

[Sady záznamů metadat](dns-zones-records.md#tags-and-metadata) lze použít tooassociate specifická data s každou sadu záznamů jako páry klíč hodnota.

Hello následující příklad ukazuje, jak nastavit toomodify hello metadata existujícího záznamu:

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


## <a name="delete-a-record-set"></a>Odstranit sadu záznamů

Sady záznamů lze odstranit pomocí hello `Remove-AzureRmDnsRecordSet` rutiny. Odstranění sady záznamů také odstraní všechny záznamy v sadě záznamů hello.

> [!NOTE]
> Nelze odstranit hello SOA a NS sady záznamů na vrcholu zóny hello (`-Name '@'`).  Azure DNS tyto vytvořena automaticky při hello zóny byl vytvořen a je při odstranění hello zóny automaticky odstraní.

Hello následující příklad ukazuje, jak toodelete sada záznamů. V tomto příkladu hello název sady záznamů, typ sady záznamů, název zóny a skupina prostředků se každý zadaný explicitně.

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Můžete taky sadu záznamů hello lze zadat pomocí názvu a typu a hello zónu pomocí zadaného objektu:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Jako třetí možnost je možné zadat samotné sady záznamů hello pomocí objekt sady záznamů:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

Když zadáte sady záznamů hello toobe odstranit pomocí objekt sady záznamů, [Etag kontroluje](dns-zones-records.md#etags) používají tooensure souběžných změny nebudou odstraněny. Můžete použít hello volitelné `-Overwrite` přepínač toosuppress těchto kontrol.

objekt sady záznamů Hello lze také přesměrovat místo předávány jako parametr:

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a>Potvrzení výzvy

Hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, a `Remove-AzureRmDnsRecordSet` rutiny všechny podporují potvrzení výzvy.

Každá rutina vyzve k potvrzení, pokud hello `$ConfirmPreference` proměnné předvoleb prostředí PowerShell má hodnotu `Medium` nebo nižší. Od hello výchozí hodnota pro `$ConfirmPreference` je `High`, tyto výzvy nejsou zadána při prostředí PowerShell výchozími nastaveními hello.

Můžete přepsat hello aktuální `$ConfirmPreference` nastavení pomocí hello `-Confirm` parametr. Pokud zadáte `-Confirm` nebo `-Confirm:$True` , hello rutina vás vyzve k potvrzení před jeho spuštění. Pokud zadáte `-Confirm:$False` , rutina hello nezobrazí výzvu k potvrzení. 

Další informace o `-Confirm` a `$ConfirmPreference`, najdete v části [o proměnné předvoleb](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Další kroky

Další informace o [zóny a záznamy v Azure DNS](dns-zones-records.md).
<br>
Zjistěte, jak příliš[chránit zóny a záznamy](dns-protect-zones-recordsets.md) při použití Azure DNS.
<br>
Zkontrolujte hello [Azure DNS PowerShell referenční dokumentaci k nástroji](/powershell/module/azurerm.dns).
