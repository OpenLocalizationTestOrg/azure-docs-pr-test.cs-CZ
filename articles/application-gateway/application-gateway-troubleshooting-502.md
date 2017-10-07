---
title: "chyby Azure Application Gateway chybný brány (502) aaaTroubleshoot | Microsoft Docs"
description: "Zjistěte, jak tootroubleshoot aplikace brány 502 chyby"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 1d431ead-d318-47d8-b3ad-9c69f7e08813
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2017
ms.author: amsriva
ms.openlocfilehash: a50f736ac157256a53f6fbe3a208f0c11dd58f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Řešení potíží s chybami Chybná brána v aplikační brány

Zjistěte, jak tootroubleshoot Chybná brána (502) chyby přijaté při použití aplikační brány.

## <a name="overview"></a>Přehled

Po dokončení konfigurace aplikační brány, jeden hello chyb, které uživatelé setkat je "Chyba serveru: 502 – Webový server obdržel neplatnou odpověď době, kdy fungoval jako brána nebo proxy server". K této chybě může dojít z důvodu následující toohello hlavních důvodů:

* Služba Azure Application Gateway [fond back-end není nakonfigurován nebo prázdný](#empty-backendaddresspool).
* Žádný z virtuálních počítačů hello nebo instancí v [Škálovací sady virtuálního počítače jsou v pořádku](#unhealthy-instances-in-backendaddresspool).
* Virtuální počítače nebo instance Škálovací sady virtuálního počítače back-end jsou [neodpovídá toohello výchozí stav kontroly](#problems-with-default-health-probe.md).
* Neplatný nebo nesprávný [konfigurace sondy vlastní stavu](#problems-with-custom-health-probe.md).
* [Žádosti o vypršení časového limitu nebo problémy s připojením k](#request-time-out) s požadavky uživatelů.

## <a name="empty-backendaddresspool"></a>Prázdný BackendAddressPool

### <a name="cause"></a>Příčina

Pokud hello Application Gateway žádné virtuální počítače nebo Škálovací sady virtuálního počítače ve fondu adres back-end hello nakonfigurován, nemůžete provádět směrování každá zákazníka žádost a vyvolá chybu Chybná brána.

### <a name="solution"></a>Řešení

Ujistěte se, že fond back-end adres hello není prázdná. To lze provést prostřednictvím prostředí PowerShell, rozhraní příkazového řádku nebo portálu.

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

výstup Hello hello předcházející rutinu by měl obsahovat fond adres back-end není prázdný. Tady je příklad, kde, dvěma fondy jsou vráceny, které jsou nakonfigurované plně kvalifikovaný název domény nebo IP adresy pro back-end virtuálních počítačů. Hello stav hello BackendAddressPool zřizování musí být 'bylo úspěšné".

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>Není v pořádku instance v BackendAddressPool

### <a name="cause"></a>Příčina

Pokud jsou všechny instance hello BackendAddressPool není v pořádku, aplikační brána nebude mít žádné back-end tooroute požadavku uživatele na. Také to může být případ hello Pokud instance back-end jsou v pořádku, ale nemají hello požadované aplikace nasazena.

### <a name="solution"></a>Řešení

Zkontrolujte, zda jsou v pořádku hello instancí a hello aplikace správně nakonfigurována. Kontrola, pokud instance back-end hello jsou možné toorespond tooa ping z jiného virtuálního počítače v hello stejnou virtuální síť. Pokud nakonfigurované veřejný koncový bod, zajistěte, aby byl prohlížeče žádost toohello webovou aplikaci obsluhovatelná.

## <a name="problems-with-default-health-probe"></a>Problémy s výchozí kontroly stavu

### <a name="cause"></a>Příčina

chyby 502 může být také časté indikátory, které hello výchozí kontroly stavu nebylo možné tooreach back-end virtuálních počítačů. Při zřizování je instance aplikační brány, výchozí stav testu tooeach BackendAddressPool automaticky konfiguruje pomocí vlastnosti hello BackendHttpSetting. Žádný uživatel vstup je požadovaná tooset tento test. Konkrétně Pokud je nakonfigurovaná pravidlo Vyrovnávání zatížení, se provádí přidružení mezi BackendHttpSetting a BackendAddressPool. Výchozí kontroly je nakonfigurován pro každý z těchto přidružení a spouští Application Gateway pravidelné stavu Kontrola připojení tooeach instance v hello BackendAddressPool v hello port zadaný v elementu BackendHttpSetting hello. Následující tabulka uvádí hello hodnoty přidružené k hello výchozí stav kontroly.

| Vlastnost testu | Hodnota | Popis |
| --- | --- | --- |
| Adresa URL testu |http://127.0.0.1/ |Cesta adresy URL |
| Interval |30 |Interval testu paměti v sekundách |
| Časový limit |30 |Časový limit testu v sekundách |
| Prahová hodnota špatného stavu |3 |Počet opakování testu. Hello back-end serveru je označena po počet po sobě jdoucích test selhání hello dosáhne hello prahová hodnota špatného stavu. |

### <a name="solution"></a>Řešení

* Zajistěte, aby výchozí web je nakonfigurován a naslouchá na 127.0.0.1.
* Pokud BackendHttpSetting určuje jiný port než 80, má být hello výchozí web nakonfigurovaných toolisten na tomto portu.
* Hello toohttp://127.0.0.1:port volání by měl vrátit kód výsledku HTTP 200. To má být vrácen v časovém limitu 30 sekund hello.
* Zkontrolujte, zda je otevřený port nakonfigurovaný a že neexistují žádná pravidla brány firewall nebo skupiny zabezpečení sítě Azure, která blokují příchozí nebo odchozí přenosy na portu hello nakonfigurované.
* Pokud virtuální počítače Azure classic nebo Cloudová služba se používá s plně kvalifikovaný název domény nebo veřejné IP adresy, ujistěte se, že hello odpovídající [koncový bod](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) je otevřen.
* Pokud hello virtuální počítač je nakonfigurován pomocí Azure Resource Manager a mimo virtuální síť, kde je nasazená aplikační bránu, hello [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) musí být nakonfigurované tooallow přístup na hello požadovaného portu.

## <a name="problems-with-custom-health-probe"></a>Problémy s vlastní stav testu

### <a name="cause"></a>Příčina

Sondy vlastní stavu Povolit výchozí toohello větší flexibilita zjišťování chování. Pokud používáte vlastní testy paměti, uživatelé můžou konfigurovat interval testu hello, hello adresu URL a cestu tootest a kolik neúspěšných odpovědí tooaccept před označením hello fond back-end instance jako chybné. budou přidány Hello následující další vlastnosti.

| Vlastnost testu | Popis |
| --- | --- |
| Name (Název) |Název testu hello. Tento název je použité toorefer toohello testu v nastavení HTTP back-end. |
| Protocol (Protokol) |Používá protokol toosend hello testu. Test Hello používá protokol hello definované v nastavení HTTP back-end hello |
| Hostitel |Hostitele název toosend hello testu. Použít pouze v případě, že na Application Gateway je nakonfigurováno více lokalit. To se liší od název hostitele virtuálního počítače. |
| Cesta |Relativní cesta hello kontroly. platná cesta Hello spustí z '/'. Test Hello je odeslán příliš\<protokol\>://\<hostitele\>:\<port\>\<cesta\> |
| Interval |Interval testu paměti v sekundách. Toto je hello časový interval mezi dvě po sobě jdoucích sondy. |
| Časový limit |Časový limit testu v sekundách. Pokud není platnou odpověď v rámci tento časový limit, hello probe je označen jako se nezdařilo. |
| Prahová hodnota špatného stavu |Počet opakování testu. Hello back-end serveru je označena po počet po sobě jdoucích test selhání hello dosáhne hello prahová hodnota špatného stavu. |

### <a name="solution"></a>Řešení

Ověřte, že hello sběru dat stavu vlastní je správně nakonfigurován jako hello předcházející tabulce. Kromě toho toohello předchozí řešení potíží, ujistěte se také hello následující:

* Ujistěte se, že test hello je správně zadán podle hello [průvodce](application-gateway-create-probe-ps.md).
* Pokud aplikace brána je nakonfigurovaná pro jednu lokalitu, ve výchozím nastavení hello hostitele název musí být zadán jako "127.0.0.1", pokud nebudou jinak nakonfigurovaná v vlastní test paměti.
* Ujistěte se, že volání toohttp: / /\<hostitele\>:\<port\>\<cesta\> vrátí výsledek kód HTTP 200.
* Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci hello přijatelný rozsah.
* Pokud sběru dat pomocí protokolu HTTPS, ujistěte se, že tento server back-end hello nevyžaduje SNI nakonfigurováním certifikát fallback na samotném serveru back-end hello. 
* Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci hello přijatelný rozsah.

## <a name="request-time-out"></a>Časový limit požadavku

### <a name="cause"></a>Příčina

Po přijetí požadavku uživatele Application Gateway platí hello nakonfigurovaná pravidla toohello požadavku a nasměruje ho fond back-end instance tooa. Čeká konfigurovat interval času pro odpověď z back-end instance hello. Ve výchozím nastavení je tento interval **30 sekund**. Pokud Application Gateway neobdrží odpověď z back-end aplikace v tomto intervalu, by žádost uživatele zobrazí chyby 502.

### <a name="solution"></a>Řešení

Aplikační brána umožňuje uživatelům tooconfigure toto nastavení prostřednictvím BackendHttpSetting, který lze potom použít toodifferent fondy. Různé fondy back-end může mít různé BackendHttpSetting a proto různé žádosti časový limit nakonfigurovaný.

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a>Další kroky

Pokud předchozí kroky hello nevyřeší hello problém, otevřete [lístek podpory](https://azure.microsoft.com/support/options/).

