---
title: "Řešení chyb Azure aplikační brány chybný brány (502) | Microsoft Docs"
description: "Zjistěte, jak vyřešit chyby 502 brány aplikace"
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
ms.openlocfilehash: cbf9c552c4818b3925f449081539f1db6d61918e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Řešení potíží s chybami Chybná brána v aplikační brány

Informace o řešení potíží s Chybná brána (502) chyby přijaté při použití aplikační brány.

## <a name="overview"></a>Přehled

Po dokončení konfigurace aplikační brány, jeden chyb, které uživatelé setkat je "Chyba serveru: 502 – Webový server obdržel neplatnou odpověď době, kdy fungoval jako brána nebo proxy server". K této chybě může dojít z následujících důvodů hlavní:

* Služba Azure Application Gateway [fond back-end není nakonfigurován nebo prázdný](#empty-backendaddresspool).
* Žádný z virtuálních počítačů nebo instancí v [Škálovací sady virtuálního počítače jsou v pořádku](#unhealthy-instances-in-backendaddresspool).
* Virtuální počítače nebo instance Škálovací sady virtuálního počítače back-end jsou [neodpovídá výchozí kontroly stavu](#problems-with-default-health-probe.md).
* Neplatný nebo nesprávný [konfigurace sondy vlastní stavu](#problems-with-custom-health-probe.md).
* [Žádosti o vypršení časového limitu nebo problémy s připojením k](#request-time-out) s požadavky uživatelů.

## <a name="empty-backendaddresspool"></a>Prázdný BackendAddressPool

### <a name="cause"></a>Příčina

Pokud službu Application Gateway nemá žádné virtuální počítače nebo sady škálování virtuálního počítače nakonfigurované ve fondu adres back-end, nemůžete provádět směrování každá zákazníka žádost a vyvolá chybu Chybná brána.

### <a name="solution"></a>Řešení

Ujistěte se, že ve fondu back-end není prázdná. To lze provést prostřednictvím prostředí PowerShell, rozhraní příkazového řádku nebo portálu.

```powershell
Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

Výstup z předchozí rutiny by měl obsahovat fond adres back-end není prázdný. Tady je příklad, kde, dvěma fondy jsou vráceny, které jsou nakonfigurované plně kvalifikovaný název domény nebo IP adresy pro back-end virtuálních počítačů. Stav zřizování BackendAddressPool musí být 'bylo úspěšné".

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

Pokud jsou všechny instance BackendAddressPool není v pořádku, aplikační brána nebude mít žádné back-end pro směrování požadavku uživatele na. Případě to mohou být také při instance back-end jsou v pořádku, ale nemáte požadovanou aplikaci nasadit.

### <a name="solution"></a>Řešení

Zajistěte, aby instance jsou v pořádku a je aplikace správně nakonfigurována. Zkontrolujte, jestli jsou instance back-end schopné reagovat na použití příkazu ping z jiného virtuálního počítače ve stejné virtuální síti. Pokud nakonfigurované veřejný koncový bod, zajistěte, aby byl žádost prohlížeče na webovou aplikaci obsluhovatelná.

## <a name="problems-with-default-health-probe"></a>Problémy s výchozí kontroly stavu

### <a name="cause"></a>Příčina

chyby 502 může být také časté indikátory, že výchozí kontroly stavu není dosažitelné virtuálních počítačů v back-end. Při zřízení instance aplikační brány automaticky nakonfiguruje výchozí kontrolu stavu pro každý BackendAddressPool pomocí vlastností BackendHttpSetting. Žádný uživatelský vstup je potřeba nastavit tento test. Konkrétně Pokud je nakonfigurovaná pravidlo Vyrovnávání zatížení, se provádí přidružení mezi BackendHttpSetting a BackendAddressPool. Výchozí kontroly je nakonfigurován pro každou z těchto přidružení a Aplikační brána zahájí pravidelné stavu zkontrolujte připojení k jednotlivým instancím v BackendAddressPool na port zadaný v elementu BackendHttpSetting. Následující tabulka uvádí hodnoty přidružené k výchozí kontroly stavu.

| Vlastnost testu | Hodnota | Popis |
| --- | --- | --- |
| Adresa URL testu |http://127.0.0.1/ |Cesta adresy URL |
| Interval |30 |Interval testu paměti v sekundách |
| Časový limit |30 |Časový limit testu v sekundách |
| Prahová hodnota špatného stavu |3 |Počet opakování testu. Back-end serverů je označena po počet po sobě jdoucích test selhání dosáhne prahová hodnota špatného stavu. |

### <a name="solution"></a>Řešení

* Zajistěte, aby výchozí web je nakonfigurován a naslouchá na 127.0.0.1.
* Pokud BackendHttpSetting určuje jiný port než 80, by měl být výchozí web nakonfigurován pro naslouchání na tomto portu.
* Volání http://127.0.0.1:port by měl vrátit kód výsledku HTTP 200. To má být vrácen v daném časovém limitu 30 sekund.
* Zkontrolujte, zda je otevřený port nakonfigurovaný a že neexistují žádná pravidla brány firewall nebo skupiny zabezpečení sítě Azure, která blokují příchozí nebo odchozí přenosy na portu nakonfigurován.
* Pokud virtuální počítače Azure classic nebo Cloudová služba se používá s plně kvalifikovaný název domény nebo veřejné IP adresy, ujistěte se, že odpovídající [koncový bod](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) je otevřen.
* Pokud virtuální počítač je nakonfigurován pomocí Azure Resource Manager a mimo virtuální síť, kde je nasazená aplikační bránu, [skupinu zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) musí být nakonfigurována pro povolení přístupu na požadovaný port.

## <a name="problems-with-custom-health-probe"></a>Problémy s vlastní stav testu

### <a name="cause"></a>Příčina

Sondy vlastní stavu povolit větší flexibilita výchozí zjišťování chování. Pokud používáte vlastní testy paměti, uživatelé můžou konfigurovat interval testu, adresu URL a cesty k testování a kolik neúspěšných odpovědí tak, aby přijímal před označením fond back-end instance jako chybné. Jsou přidány následující další vlastnosti.

| Vlastnost testu | Popis |
| --- | --- |
| Name (Název) |Název kontroly. Tento název se používá k odkazování na test v nastavení HTTP back-end. |
| Protocol (Protokol) |Protokol používaný k odesílání sonda. Tato kontrola používá protokol definované v nastavení HTTP back-end |
| Hostitel |Název hostitele k odeslání test. Použít pouze v případě, že na Application Gateway je nakonfigurováno více lokalit. To se liší od název hostitele virtuálního počítače. |
| Cesta |Relativní cesta kontroly. Platná cesta spustí z '/'. Posílá sonda \<protokol\>://\<hostitele\>:\<port\>\<cesta\> |
| Interval |Interval testu paměti v sekundách. Toto je časový interval mezi dvě po sobě jdoucích sondy. |
| Časový limit |Časový limit testu v sekundách. Pokud není platnou odpověď v rámci tento časový limit, tato kontrola je označeno jeho selhání. |
| Prahová hodnota špatného stavu |Počet opakování testu. Back-end serverů je označena po počet po sobě jdoucích test selhání dosáhne prahová hodnota špatného stavu. |

### <a name="solution"></a>Řešení

Ověřte, zda vlastní stav testu správně nakonfigurovaná jako v předchozí tabulce. Kromě předchozích kroků řešení potíží zkontrolujte také následující:

* Zajistěte, aby tato kontrola je správně zadán dle [průvodce](application-gateway-create-probe-ps.md).
* Pokud aplikace brána je nakonfigurovaná pro jednu lokalitu, ve výchozím nastavení hostitele název musí být zadán jako "127.0.0.1", pokud nebudou jinak nakonfigurovaná v vlastní test paměti.
* Ujistěte se, že volání http://\<hostitele\>:\<port\>\<cesta\> vrátí výsledek kód HTTP 200.
* Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci přijatelný rozsah.
* Pokud sběru dat pomocí protokolu HTTPS, ujistěte se, že back-end serveru nevyžaduje SNI nakonfigurováním certifikát fallback na samotném serveru back-end. 
* Zajistěte, aby Interval, časový limit a UnhealtyThreshold v rámci přijatelný rozsah.

## <a name="request-time-out"></a>Časový limit požadavku

### <a name="cause"></a>Příčina

Po přijetí požadavku uživatele Aplikační brána nakonfigurovaná pravidla se vztahují na žádost a směruje na fond back-end instance. Čeká konfigurovat interval času pro odpověď z back-end instance. Ve výchozím nastavení je tento interval **30 sekund**. Pokud Application Gateway neobdrží odpověď z back-end aplikace v tomto intervalu, by žádost uživatele zobrazí chyby 502.

### <a name="solution"></a>Řešení

Aplikační brána umožňuje uživatelům nakonfigurovat toto nastavení prostřednictvím BackendHttpSetting, který lze potom použít na různých fondů. Různé fondy back-end může mít různé BackendHttpSetting a proto různé žádosti časový limit nakonfigurovaný.

```powershell
    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="next-steps"></a>Další kroky

Pokud předchozí kroky není problém vyřešit, otevřete [lístek podpory](https://azure.microsoft.com/support/options/).

