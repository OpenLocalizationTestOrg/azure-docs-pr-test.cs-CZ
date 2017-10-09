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
# <a name="delegate-a-domain-tooazure-dns"></a>Delegát tooAzure domény DNS

Azure DNS vám umožní toohost zóny DNS a spravovat hello záznamy DNS pro doménu v Azure. Aby dotazy DNS pro domény tooreach Azure DNS, má doména hello toobe delegovaná z nadřazené domény hello tooAzure DNS. Mějte na paměti Azure DNS není doménový Registrátor hello. Tento článek vysvětluje, jak toodelegate tooAzure vaší domény DNS.

U domén zakoupených od doménového registrátora registrátora nabízí možnost tooset hello si tyto záznamy NS. Nemáte tooown domény toocreate zónu DNS s tímto názvem domény v Azure DNS. Je však nutné, aby tooown hello domény tooset až tooAzure hello delegování DNS u registrátora hello.

Předpokládejme například, nákupu hello domény "contoso.net" a v Azure DNS vytvoříte zónu s názvem hello "contoso.net". Jako vlastník hello hello domény vašeho registrátora nabízí že Hello možnost tooconfigure hello adresy názvových serverů (tedy záznamy hello NS) pro doménu. Hello Registrátor uloží tyto záznamy NS v nadřazené doméně hello, v tomto případě ".net.. Klienti kolem hello, world pak lze směrovanou tooyour doménu v zóně Azure DNS, při pokusu o tooresolve záznamů DNS v "contoso.net".

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se toohello portálu Azure
1. V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.

    ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.net|Název zóny DNS hello Hello|
   |**Předplatné**|[Vaše předplatné]|Vyberte bránu předplatné toocreate hello aplikace v.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoRG|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello. umístění zóny DNS Hello je vždy "globální" a není zobrazen.

## <a name="retrieve-name-servers"></a>Načtení názvových serverů

Předtím, než můžete delegovat vaše tooAzure DNS zóny DNS, musíte nejprve názvy tooknow hello názvových serverů zóny. Azure DNS přiděluje názvové servery z fondu vždy, když je vytvořena zóna.

1. S zóny DNS hello vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **contoso.net** zónu DNS v hello **všechny prostředky** okno. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **contoso.net** v hello filtr podle názvu... pole tooeasily přístup hello aplikační brány. 

1. V okně zóny DNS hello načíst hello názvové servery. V tomto příkladu hello zónu "contoso.net" přiřazené názvové servery ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS.NET', ' ns3-01.azure-dns.org', a ' ns4-01.azure-dns.info':

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří záznamy autoritativních NS ve vaší zóně obsahující hello přiřazené názvové servery.  názvy toosee hello název serveru pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure, jednoduše musíte tooretrieve tyto záznamy.

Hello následující příklady také popisují hello tooretrieve hello názvové servery pro zónu v Azure DNS pomocí prostředí PowerShell a rozhraní příkazového řádku Azure.

### <a name="powershell"></a>PowerShell

```powershell
# hello record name "@" is used toorefer toorecords at hello top of hello zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

Následující ukázka Hello je hello odpovědi.

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

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

Následující ukázka Hello je hello odpovědi.

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

## <a name="delegate-hello-domain"></a>Delegát hello domény

Teď, když je vytvořena zóna DNS hello a máte hello názvové servery, musí hello nadřazené domény toobe aktualizovat pomocí názvových serverů Azure DNS hello. Každý Registrátor má vlastní DNS správy nástroje toochange hello záznamy názvového serveru pro doménu. Na stránce správy DNS vašeho registrátora hello upravit záznamy NS hello a nahraďte záznamy NS hello hello ty, které vytvořil Azure DNS.

Při delegování domény tooAzure DNS, musíte použít názvy názvových serverů hello poskytuje Azure DNS. Je doporučeno toouse všechny čtyři název názvů serverů, bez ohledu na to hello název vaší domény. Delegování domény nevyžaduje hello název serveru název toouse hello stejné domény nejvyšší úrovně jako doménu.

Neměli byste používat "spojovací záznamy" toopoint toohello Azure DNS název IP adresy serverů, protože tyto IP adresy mohou v budoucnu měnit. Delegování pomocí názvů názvových serverů ve vaší vlastní zóně, někdy označovaných jako „jednoduché názvové servery“, v současné době není v Azure DNS podporované.

## <a name="verify-name-resolution-is-working"></a>Ověření, že překlad názvů funguje

Po dokončení hello delegování, můžete ověřit, že funguje překlad adres pomocí nástroje, jako je například "nslookup" tooquery hello záznam SOA pro vaši zónu (ten je také automaticky vytvořený při vytváření zóny hello).

Nemají názvových serverů Azure DNS hello toospecify, pokud hello delegování byla nastavena správně, hello normální proces překladu DNS nalezne názvové servery hello automaticky.

```
nslookup -type=SOA contoso.com
```

Hello následuje odpověď příklad z hello předcházející příkaz:

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

## <a name="delegate-sub-domains-in-azure-dns"></a>Delegování subdomén v Azure DNS

Pokud chcete tooset až samostatnou podřízenou zónu, můžete delegovat subdomény v Azure DNS. Například s nastavit a delegované "contoso.net" v Azure DNS Předpokládejme, že byste chtěli tooset až samostatnou podřízenou zónu, 'partners.contoso.net'.

1. Partners.contoso.net' hello podřízenou zónu' vytvořte v Azure DNS.
2. Vyhledejte záznamy autoritativních NS hello v hello podřízené zóny tooobtain hello názvové servery hostující podřízenou zónu hello v Azure DNS.
3. Delegát hello podřízenou zónu pomocí konfigurace záznamů NS v nadřazené zóně hello odkazující toohello podřízenou zónu.

### <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se toohello portálu Azure
1. V nabídce centra hello, klikněte na tlačítko a klikněte na tlačítko **nový > sítě >** a pak klikněte na **zónu DNS** tooopen hello vytvoření DNS zóny okno.

    ![Zóna DNS](./media/dns-domain-delegation/dns.png)

1. Na hello **zóny DNS vytvořte** okno Zadejte hello následující hodnoty a pak klikněte na **vytvořit**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|partners.contoso.net|Název zóny DNS hello Hello|
   |**Předplatné**|[Vaše předplatné]|Vyberte bránu předplatné toocreate hello aplikace v.|
   |**Skupina prostředků**|**Použít existující:** contosoRG|Vytvořte skupinu prostředků. Název skupiny prostředků Hello musí být jedinečný v rámci předplatného hello, který jste vybrali. Další informace o skupinách prostředků, přečtěte si hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) článek s přehledem.|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků Hello odkazuje toohello umístění skupiny prostředků hello a nemá žádný vliv na zónu DNS hello. umístění zóny DNS Hello je vždy "globální" a není zobrazen.

### <a name="retrieve-name-servers"></a>Načtení názvových serverů

1. S zóny DNS hello vytvořené v hello portál Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **partners.contoso.net** zónu DNS v hello **všechny prostředky** okno. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **partners.contoso.net** v hello filtr podle názvu... pole tooeasily přístup hello DNS zóny.

1. V okně zóny DNS hello načíst hello názvové servery. V tomto příkladu hello zónu "contoso.net" přiřazené názvové servery ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS.NET', ' ns3-01.azure-dns.org', a ' ns4-01.azure-dns.info':

 ![Názvový server DNS](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS automaticky vytvoří záznamy autoritativních NS ve vaší zóně obsahující hello přiřazené názvové servery.  názvy toosee hello název serveru pomocí prostředí Azure PowerShell nebo rozhraní příkazového řádku Azure, jednoduše musíte tooretrieve tyto záznamy.

### <a name="create-name-server-record-in-parent-zone"></a>Vytvoření záznamu názvového serveru v nadřazené zóně

1. Přejděte toohello **contoso.net** zónu DNS v hello portálu Azure.
1. Klikněte na **+ Sada záznamů**.
1. Na hello **přidat sadu záznamů** okno, zadejte následující hodnoty hello a pak klikněte na **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|partners|Název zóny DNS podřízené hello Hello|
   |**Typ**|NS|Pro záznamy názvového serveru použijte NS.|
   |**Hodnota TTL**|1|Čas toolive.|
   |**Jednotka hodnoty TTL**|Hodiny|Nastaví toohours toolive jednotka času|
   |**NÁZVOVÝ SERVER**|{názvové servery ze zóny partners.contoso.net}|Zadejte všechny 4 hello názvové servery z partners.contoso.net zóny. |

   ![Názvový server DNS](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Delegování subdomén v Azure DNS pomocí jiných nástrojů

Hello následující příklady popisují hello toodelegate subdomén v Azure DNS pomocí prostředí PowerShell a rozhraní příkazového řádku:

#### <a name="powershell"></a>PowerShell

Hello následující příklad PowerShell ukazuje, jak to funguje. Hello stejný postup lze provést prostřednictvím hello portál Azure nebo prostřednictvím hello rozhraní příkazového řádku Azure napříč platformami.

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

Použití `nslookup` tooverify, které všechno, co jsou nastaveny správně vyhledáním záznamu SOA podřízené zóny hello hello.

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

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Načtení hello názvové servery pro hello `partners.contoso.net` zóny z výstupu hello.

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

Vytvořte hello sady záznamů a záznamy NS pro každý název serveru.

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

## <a name="delete-all-resources"></a>Odstranění všech prostředků

toodelete vytvořit všechny prostředky v tomto článku, dokončení hello následující kroky:

1. V portálu Azure hello **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte na tlačítko hello **contosorg** skupina prostředků v hello okno všechny prostředky. Pokud jste vybrali, již předplatné hello neobsahuje několik prostředků, můžete zadat **contosorg** v hello **filtrovat podle názvu...** pole tooeasily přístup hello prostředků skupiny.
1. V hello **contosorg** okně klikněte na tlačítko hello **odstranit** tlačítko.
1. Hello portál vyžaduje tootype hello název hello prostředků skupiny tooconfirm, které chcete toodelete ho. Typ *contosorg* hello název skupiny prostředků, klikněte **odstranit**. Odstranění skupiny prostředků se odstraní všechny prostředky v rámci skupiny prostředků hello, takže vždy být zda tooconfirm hello obsah skupinu prostředků. před odstraněním. Odstraní všechny prostředky obsažené v rámci skupiny prostředků hello Hello portálu, a pak odstraní samotná skupina prostředků hello. Tento proces trvá několik minut.

## <a name="next-steps"></a>Další kroky

[Správa zón DNS](dns-operations-dnszones.md)

[Správa záznamů DNS](dns-operations-recordsets.md)
