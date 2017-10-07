---
title: "aaaCreate vlastní záznamy DNS pro webovou aplikaci | Microsoft Docs"
description: "Jak záznamy toocreate vlastní domény DNS pro webovou aplikaci pomocí Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 070c808a55bab922eb624d99ae5c275d8eaa5aaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Vytvořit záznamy DNS pro webovou aplikaci ve vlastní domény

Azure DNS toohost vlastní doménu můžete použít pro webové aplikace. Například při vytváření webové aplikace Azure a chcete, aby vaši uživatelé tooaccess ho a to buď pomocí contoso.com nebo www.contoso.com jako plně kvalifikovaný název domény.

toodo, máte dva záznamy toocreate:

* Kořenový "A" záznamu polohovací toocontoso.com
* "Záznam CNAME" záznam pro hello www název, který ukazuje toohello záznam

Uvědomte si, pokud vytvoříte záznam pro webovou aplikaci v Azure, hello záznam je potřeba aktualizovat ručně pokud hello Zdrojová IP adresa pro změny hello webové aplikace.

## <a name="before-you-begin"></a>Než začnete

Než začnete, musíte nejprve vytvořit zónu DNS v Azure DNS a delegovat hello zóny v tooAzure vašeho registrátora DNS.

1. toocreate zóny DNS, postupujte podle kroků hello v [vytvořit zónu DNS](dns-getstarted-create-dnszone.md).
2. toodelegate vaše DNS tooAzure DNS, postupujte podle kroků hello v [delegování DNS domény](dns-domain-delegation.md).

Po vytvoření zóny a delegování ho tooAzure DNS, potom můžete vytvořit záznamy pro vaši vlastní doménu.

## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Vytvoření záznamu A pro vaši vlastní doménu.

Záznam je použité toomap na název tooits IP adresu. V následující ukázka hello jsme se přiřadit jako tooan záznam A adresu IPv4:

### <a name="step-1"></a>Krok 1

Vytvořte záznam a přiřaďte tooa proměnné $rs

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a>Krok 2

Přidat hello IPv4 hodnota toohello dříve vytvořili sadu záznamů "@" použití hello $rs proměnné přiřazené. Hello IPv4 přiřazené bude hello IP adresu pro vaši webovou aplikaci.

toofind hello IP adres pro webovou aplikaci, postupujte podle kroků hello v [konfigurace vlastního názvu domény v Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a>Krok 3

Potvrďte hello změny toohello sady záznamů. Použití `Set-AzureRMDnsRecordSet` tooupload hello změny tooAzure toohello sada záznamů DNS:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Vytvořte záznam CNAME pro vaši vlastní doménu.

Pokud vaše doména je již spravován nástrojem Azure DNS (viz [delegování DNS domény](dns-domain-delegation.md), můžete použít následující příklad toocreate hello záznam CNAME pro contoso.azurewebsites.net hello.

### <a name="step-1"></a>Krok 1

Otevřete prostředí PowerShell a vytvořte novou sadu záznamů CNAME a přiřaďte tooa proměnné $rs. Tento příklad vytvoří sadu záznamů typu CNAME s "časem toolive" 600 sekund v zóně DNS s názvem "contoso.com".

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

Následující ukázka Hello je hello odpovědi.

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>Krok 2

Po vytvoření sady záznamů CNAME hello musíte toocreate hodnotou alias, který bude odkazovat toohello webové aplikace.

Hello dřív přiřazené proměnná "$rs" pomocí příkazu prostředí PowerShell hello níže toocreate hello alias pro hello webové aplikace contoso.azurewebsites.net.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

Následující ukázka Hello je hello odpovědi.

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>Krok 3

Potvrzení změn hello pomocí hello `Set-AzureRMDnsRecordSet` rutiny:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

Můžete ověřit hello záznam vytvořil správně dotazováním hello "www.contoso.com" pomocí nástroje nslookup, jak je uvedeno níže:

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a>Vytvoření záznamů "awverify" pro webové aplikace

Pokud se rozhodnete toouse záznam pro vaši webovou aplikaci, musíte přejít do tooensure ověření procesu můžete vlastní hello vlastní doménu. Tento krok ověření se provádí tak, že vytvoříte speciální záznam CNAME s názvem "awverify". Tato část se týká pouze tooA záznamy.

### <a name="step-1"></a>Krok 1

Vytvořte záznam "awverify" hello. V příkladu hello dole vytvoříme hello "aweverify" záznam pro contoso.com tooverify vlastnictví pro vlastní doménu, hello.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

Následující ukázka Hello je hello odpovědi.

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>Krok 2

Po vytvoření sady záznamů hello "awverify" přiřadíte alias sadu záznamů CNAME hello. V příkladu hello níže jsme přiřadí tooawverify.contoso.azurewebsites.net alias sady záznamů CNAMe hello.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

Následující ukázka Hello je hello odpovědi.

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>Krok 3

Potvrzení změn hello pomocí hello `Set-AzureRMDnsRecordSet cmdlet`, jak ukazuje následující příkaz hello.

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a>Další kroky

Postupujte podle kroků hello v [konfigurace vlastního názvu domény pro službu App Service](../app-service-web/web-sites-custom-domain-name.md) tooconfigure vaší webové aplikace toouse vlastní doménu.
