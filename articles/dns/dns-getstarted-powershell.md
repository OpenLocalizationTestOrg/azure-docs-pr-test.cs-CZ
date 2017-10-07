---
title: "aaaGet začít s Azure DNS pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak toocreate DNS zóna a záznam v Azure DNS. Toto je podrobný průvodce toocreate a spravovat vaše první zónu DNS a zaznamenejte pomocí prostředí PowerShell."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 0f9dead1e4b44fcc74c84a024c41cdfaeb02b5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a>Začínáme s DNS Azure pomocí PowerShellu

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Tento článek vás provede kroky toocreate hello první zónu DNS a záznam pomocí Azure PowerShell. Můžete také provést tyto kroky, pomocí hello portál Azure nebo hello rozhraní příkazového řádku Azure napříč platformami.

Zóny DNS je použité toohost hello záznamy DNS pro konkrétní domény. toostart hostovat svoji doménu v Azure DNS, je nutné toocreate zóny DNS pro název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Nakonec toopublish DNS zóny toohello Internet, musíte tooconfigure hello názvové servery pro doménu hello. Jednotlivé kroky jsou popsány níže.

Tyto pokyny předpokládají, je již nainstalován a přihlášení tooAzure prostředí PowerShell. Nápovědu najdete v tématu [jak toomanage DNS zóny pomocí prostředí PowerShell](dns-operations-dnszones.md).

## <a name="create-hello-resource-group"></a>Vytvořte skupinu prostředků hello

Před vytvořením zóny DNS hello, skupinu prostředků se vytvoří zónu DNS toocontain hello. Následující Hello ukazuje příkaz hello.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

Zóny DNS je vytvořená pomocí hello `New-AzureRmDnsZone` rutiny. Hello následující příklad vytvoří zónu DNS s názvem *contoso.com* v hello skupinu prostředků s názvem *MyResourceGroup*. Použijte příklad hello toocreate zóny DNS, nahraďte hello hodnoty vlastními.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

Vytvoření sady záznamů pomocí hello `New-AzureRmDnsRecordSet` rutiny. Hello následující příklad vytvoří záznam s relativním názvem "www" v zóně DNS "contoso.com" ve skupině prostředků "MyResourceGroup" hello hello. Hello plně kvalifikovaný název sady záznamů hello je "www.contoso.com". Typ záznamu Hello je "A" s IP adresou "1.2.3.4" a je hello TTL 3 600 sekund.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

U jiných typů záznamů s více než jeden záznam a existující záznamy toomodify sady záznamů, najdete v části [záznamy DNS spravovat a sad záznamů pomocí Azure PowerShell](dns-operations-recordsets.md). 


## <a name="view-records"></a>Zobrazení záznamů

toolist hello záznamy DNS v zóně, použijte:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Aktualizace názvových serverů

Jakmile jste uspokojit, že zóna DNS a záznamy byly nastaveny správně, musíte tooconfigure váš název domény toouse názvových serverů Azure DNS hello. To umožňuje jiných uživatelů v Internetu toofind hello svoje záznamy DNS.

Hello názvové servery zóny jsou poskytují hello `Get-AzureRmDnsZone` rutiny:

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Tyto názvové servery musí být nakonfigurovaný u registrátora názvu domény hello (kde jste zakoupili hello název domény). Vám Registrátor nabídne možnost tooset hello až hello názvové servery pro doménu hello. Další informace najdete v tématu [delegovat tooAzure vaší domény DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Odstranění všech prostředků

toodelete vytvořit všechny prostředky v tomto článku, hello proveďte následující krok:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Další kroky

toolearn Další informace o Azure DNS, najdete v části [přehled Azure DNS](dns-overview.md).

toolearn Další informace o správě zóny DNS v Azure DNS, najdete v části [zóny DNS spravovat v Azure DNS pomocí prostředí PowerShell](dns-operations-dnszones.md).

toolearn Další informace o správě záznamů DNS v Azure DNS, najdete v části [sad záznamů a záznamů DNS spravovat v Azure DNS pomocí prostředí PowerShell](dns-operations-recordsets.md).
