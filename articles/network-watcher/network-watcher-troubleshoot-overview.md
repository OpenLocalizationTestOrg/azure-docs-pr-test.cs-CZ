---
title: "řešení potíží s v sledovací proces sítě Azure tooresource aaaIntroduction | Microsoft Docs"
description: "Tato stránka obsahuje přehled řešení problémů s možností prostředků hello sledovací proces sítě"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: ccbe4c1c2364473aba06e709460d67c773cf25ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooresource-troubleshooting-in-azure-network-watcher"></a>Řešení potíží s v sledovací proces sítě Azure tooresource Úvod

Brány virtuální sítě zajistěte připojení mezi místními prostředky a dalším virtuálním sítím v rámci Azure. Monitorování tyto brány a jejich připojení je velmi důležité tooensuring komunikace není poškozený. Sledovací proces sítě poskytuje schopnost tootroubleshoot hello brány virtuální sítě a připojení. To je možné volat prostřednictvím portálu hello, prostředí PowerShell, rozhraní příkazového řádku nebo REST API. Při volání, sledovací proces sítě diagnostikuje hello stavu brány virtuální sítě hello nebo připojení a příslušné výsledky vrácené hello. Tento požadavek je dlouhotrvající transakci, hello výsledky, se vrátí po dokončení diagnostiky hello.

![portál][2]

## <a name="results"></a>Výsledky

Hello předběžné výsledky vrácené poskytují celkový přehled o stavu hello hello prostředku. Podrobnější informace lze zadat pro prostředky, jak je znázorněno v následující části hello:

Hello následujícím seznamu je hello hodnot vrácených s hello řešení rozhraní API:

* **startTime** – tato hodnota je čas hello hello řešení volání rozhraní API spustit.
* **čas ukončení** – tato hodnota je hello čas ukončení řešení potíží s hello.
* **kód** – tato hodnota není v pořádku, pokud dojde k selhání jedné diagnostiku.
* **výsledky** -výsledků je kolekce výsledků vrácených na hello připojení nebo hello brány virtuální sítě.
    * **ID** – tato hodnota je typem chyby hello.
    * **Souhrn** – tato hodnota je uveden seznam hello selhání.
    * **Podrobné** -tuto hodnotu poskytuje podrobný popis chyby hello.
    * **recommendedActions** -tato vlastnost je kolekce tootake doporučené akce.
      * **actionText** – tato hodnota obsahuje text hello popisující, co tootake akce.
      * **actionUri** -tuto hodnotu poskytuje hello URI toodocumentation tooact.
      * **actionUriText** – tato hodnota je krátký popis text hello akce.

Následující tabulky zobrazit hello různé chyby typy (id pod výsledky z hello předcházející seznamu), které jsou k dispozici Hello a pokud hello selhání vytvoří protokoly.

### <a name="gateway"></a>brána

| Typ chyby | Důvod | Protokol|
|---|---|---|
| NoFault | Když je zjištěna žádná chyba. |Ano|
| GatewayNotFound | Nelze najít, že není zřízený brány nebo brána. |Ne|
| PlannedMaintenance |  Instance brány je v rámci údržby.  |Ne|
| UserDrivenUpdate | Pokud je aktualizace uživatele v průběhu. To může být operace změny velikosti. | Ne |
| VipUnResponsive | Nelze kontaktovat hello primární instance hello brány. To se stane, když selže test stavu hello. | Ne |
| PlatformInActive | Nastane problém s platformou hello. | Ne|
| ServiceNotRunning | Hello základní služba není spuštěna. | Ne|
| NoConnectionsFoundForGateway | Žádná připojení existuje v bráně hello. Toto je pouze upozornění.| Ne|
| ConnectionsNotConnected | Připojení nejsou připojené. Toto je pouze upozornění.| Ano|
| GatewayCPUUsageExceeded | Aktuální využití procesoru brány Hello je > 95 %. | Ano |

### <a name="connection"></a>Připojení

| Typ chyby | Důvod | Protokol|
|---|---|---|
| NoFault | Když je zjištěna žádná chyba. |Ano|
| GatewayNotFound | Nelze najít, že není zřízený brány nebo brána. |Ne|
| PlannedMaintenance | Instance brány je v rámci údržby.  |Ne|
| UserDrivenUpdate | Pokud je aktualizace uživatele v průběhu. To může být operace změny velikosti.  | Ne |
| VipUnResponsive | Nelze kontaktovat hello primární instance hello brány. Ho se stane, když selže test stavu hello. | Ne |
| ConnectionEntityNotFound | Konfigurace připojení nebyl nalezen. | Ne |
| ConnectionIsMarkedDisconnected | Hello připojení je označena jako "odpojené". |Ne|
| ConnectionNotConfiguredOnGateway | základní služby Hello nemá hello nakonfigurováno připojení. | Ano |
| ConnectionMarkedStandy | podkladová služba Hello je označena jako pohotovostní režim.| Ano|
| Authentication | Neshoda předsdílený klíč. | Ano|
| PeerReachability | Hello sdílené brány není dostupný. | Ano|
| IkePolicyMismatch | Hello sdílené brány má IKE zásady, které nejsou podporované službou Azure. | Ano|
| Chyba WfpParse | Došlo k chybě analýzy protokolů Ochrana souborů systému Windows hello. |Ano|

## <a name="supported-gateway-types"></a>Podporované typy brány

Hello následující seznam obsahuje podporu hello ukazuje připojení a bran, které jsou podporovány při řešení problémů sledovací proces sítě.
|  |  |
|---------|---------|
|**Typy brány**   |         |
|Síť VPN      | Podporuje se        |
|ExpressRoute | Nepodporuje se |
|Hypernet | Nepodporuje se|
|**Typy sítě VPN** | |
|Na základě trasy | Podporuje se|
|Na základě zásad | Nepodporuje se|
|**Typy připojení**||
|Protokol IPSec| Podporuje se|
|VNet2Vnet| Podporuje se|
|ExpressRoute| Nepodporuje se|
|Hypernet| Nepodporuje se|
|VPNClient| Nepodporuje se|

## <a name="log-files"></a>Soubory protokolu

soubory protokolů Poradce prostředků Hello jsou uložené v účtu úložiště po dokončení řešení potíží s prostředků. Hello následující obrázek znázorňuje příklad obsah hello volání, jehož výsledkem chyba.

![soubor ZIP][1]

> [!NOTE]
> V některých případech je zapsán pouze podmnožinu souborů protokolů hello toostorage.

Pokyny ke stahování souborů z účty azure storage, najdete v části příliš[Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Jiný nástroj, který je možné je Storage Explorer. Další informace o Storage Explorer naleznete zde na hello následující odkaz: [Storage Explorer](http://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

Hello **ConnectionStats.txt** soubor obsahuje celkové statistiky hello připojení, včetně příchozí a Odchozí bajty, stav připojení a hello čas hello bylo vytvořeno připojení.

> [!NOTE]
> Pokud toohello volání hello řešení potíží s rozhraní API vrátí v pořádku, je hello pouze věcí, vrátí se v souboru zip hello **ConnectionStats.txt** souboru.

Hello obsah tohoto souboru jsou podobné toohello následující ukázka:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

Hello **CPUStats.txt** soubor obsahuje využití procesoru a paměti, které jsou k dispozici v době hello testování.  Hello obsah tohoto souboru je podobné toohello následující ukázka:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

Hello **IKEErrors.txt** soubor obsahuje chyby IKE, ke kterým během monitorování nebyly nalezeny.

Hello následující příklad ukazuje hello obsahu souboru IKEErrors.txt. Vaše chyby se může lišit v závislosti na hello problém.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Očistí wfpdiag.txt

Hello **Scrubbed wfpdiag.txt** protokolový soubor obsahuje hello wfp protokolu. Tento protokol obsahuje protokolování paketu vyřaďte a IKE/AuthIP selhání.

Hello následující příklad ukazuje hello obsah souboru Scrubbed wfpdiag.txt hello. V tomto příkladu nebyla hello sdílený klíč připojení správné, jak je vidět z hello 3. řádku zdola hello. Následující ukázka Hello je právě fragment hello celý protokolu, jako hello protokolu může být náročná v závislosti na hello problém.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from hello high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.Sum

Hello **wfpdiag.txt.sum** je soubor protokolu hello vyrovnávací paměti a zpracovaných událostí.

Hello následující příklad je hello obsah souboru wfpdiag.txt.sum.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toodiagnose brány sítě VPN a připojení přes hello navštivte stránky portálu [brány řešení potíží – portál Azure](network-watcher-troubleshoot-manage-portal.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
