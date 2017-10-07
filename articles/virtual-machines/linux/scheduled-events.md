---
title: "aaaScheduled události pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Naplánované událostí pomocí služby Azure metadat hello pro na virtuální počítače s Linuxem."
services: virtual-machines-windows, virtual-machines-linux, cloud-services
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: zivr
ms.openlocfilehash: e153c8854725330acefd67d0ef542f6149170a7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-metadata-service-scheduled-events-preview-for-linux-vms"></a>Služba Azure Metadata: Naplánované události (Preview) pro virtuální počítače s Linuxem

> [!NOTE] 
> Verze Preview, jsou k dispozici tooyou probíhají hello podmínky, že souhlasíte toohello podmínky použití. Další informace najdete v [dodatečných podmínkách použití systémů Microsoft Azure Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Naplánované události je jedním z subservices hello pod hello Metadata služby Azure. Zodpovídá za zpřístupnění informací o nadcházející události (například restartování) tak, aby vaše aplikace můžete připravit pro ně a omezit přerušení. Je k dispozici pro všechny typy virtuálního počítače Azure, včetně PaaS a IaaS. Naplánované události dává preventivní úlohami virtuálního počítače čas tooperform toominimize hello účinku událost. 

Naplánované události je k dispozici pro systém Windows a virtuální počítače s Linuxem. Informace o naplánované události v systému Windows najdete v tématu [naplánované události pro virtuální počítače Windows](../windows/scheduled-events.md).

## <a name="why-scheduled-events"></a>Proč naplánované události?

Naplánované události je můžete provést kroky toolimit hello dopad platformy intiated údržby nebo akce zahájená uživatelem vaší služby. 

S více instancemi úloh, které použití replikace techniky toomaintain stavu, může být ohrožena toooutages děje ve více instancích. Například výpadky může vést k nákladné úlohy (například rekonstrukci indexy) nebo i ke ztrátě replik. 

V mnoha jiných případech hello celkové dostupnosti služby, je možné zvýšit provedením řádné vypnutí pořadí dokončuje (nebo ruší) během letu transakce, přeřazení tooother úlohy virtuálních počítačů v clusteru hello (ruční převzetí služeb při selhání), nebo odebrání hello Virtuální počítač z fondu vyrovnávání zatížení sítě. 

Existují případy, kdy se žádat o pomoc správce o nadcházející události nebo protokolování takové události pomoci, vylepšení hello použitelnost aplikací hostovaných v cloudu hello.

Případy použití Azure Metadata povrchy naplánované události služby v hello následující:
-   Platforma iniciované údržby (například zavádění hostitelským operačním systémem)
-   Uživatel spustil volání (například restartování uživatele nebo opětovně nasadí virtuální počítač)


## <a name="hello-basics"></a>Základy Hello  

Služba Azure Metadata zpřístupní informace o spouštění virtuálních počítačů pomocí koncový bod REST, která je přístupná z v rámci hello virtuálních počítačů. Hello informace jsou k dispozici prostřednictvím směrovat IP tak, aby není nezveřejní hello virtuálních počítačů.

### <a name="scope"></a>Rozsah
Naplánované události jsou prezentované tooall virtuální počítače v cloudové službě nebo tooall virtuální počítače ve skupině dostupnosti. V důsledku toho byste měli zkontrolovat hello `Resources` pole tooidentify hello událostí, které virtuální počítače se bude dopad na toobe. 

### <a name="discovering-hello-endpoint"></a>Koncový bod zjišťování hello
V případě hello, kde se má vytvořit virtuální počítač v rámci virtuální sítě (VNet), je k dispozici ze statické IP adresy směrovat, služba metadat hello `169.254.169.254`.
Pokud není vytvořená hello virtuálního počítače v rámci virtuální sítě, hello výchozí případů pro cloudové služby a klasické virtuální počítače, je další logiku toouse koncového bodu vyžaduje toodiscover hello. Jak příliš najdete ukázkové toolearn toothis[zjistit koncový bod hostitele hello](https://github.com/azure-samples/virtual-machines-python-scheduled-events-discover-endpoint-for-non-vnet-vm).

### <a name="versioning"></a>Správa verzí 
Hello Instance služby metadat je verzí. Verze jsou povinné a hello aktuální verze je `2017-03-01`.

> [!NOTE] 
> Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako hello api-version. Tento formát se už nepodporuje a bude v budoucí hello nepoužívá.

### <a name="using-headers"></a>Používání hlaviček
Když dotazujete hello Metadata služby, je nutné zadat hello záhlaví `Metadata: true` tooensure hello požadavek nebyl přesměrován náhodně.

### <a name="enabling-scheduled-events"></a>Povolení naplánované události
Hello poprvé, co musíte provést žádost pro naplánované události Azure implicitně povolí hello funkci na virtuálním počítači. V důsledku toho byste měli očekávat zpožděné odpovědi v prvním volání z až tootwo minut.

### <a name="user-initiated-maintenance"></a>Údržby iniciované uživatelem
Uživatel spustil údržby virtuálního počítače prostřednictvím hello portál Azure, rozhraní API, rozhraní příkazového řádku, nebo prostředí PowerShell, které jsou výsledkem plánovaná událost. To vám umožní tootest hello údržby přípravy logiku v aplikaci a umožňuje tooprepare vaší aplikace pro údržbu inicializované uživatelem.

Restartování virtuálního počítače plány událost s typem `Reboot`. Opětovné nasazení virtuálního počítače plány událost s typem `Redeploy`.

> [!NOTE] 
> Aktuálně může být současně naplánována maximálně 10 operací údržby inicializované uživatelem. Tento limit bude zmírnit před naplánované události obecné dostupnosti.

> [!NOTE] 
> Údržby iniciované uživatelem, což vede k naplánované události se momentálně nedá konfigurovat. Možnosti konfigurace: je plánovaná pro budoucí použití.

## <a name="using-hello-api"></a>Pomocí rozhraní API hello

### <a name="query-for-events"></a>Dotaz pro události
Pro naplánované události můžete dotazovat jednoduše tak, že hello následující volání:

```
curl -H Metadata:true http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

Odpověď obsahuje pole naplánované události. Prázdné pole znamená, že aktuálně neexistují žádné události naplánované.
V případě hello tam, kde jsou naplánované události, hello odpovědi obsahuje řadu událostí: 
```
{
    "DocumentIncarnation": {IncarnationID},
    "Events": [
        {
            "EventId": {eventID},
            "EventType": "Reboot" | "Redeploy" | "Freeze",
            "ResourceType": "VirtualMachine",
            "Resources": [{resourceName}],
            "EventStatus": "Scheduled" | "Started",
            "NotBefore": {timeInUTC},              
        }
    ]
}
```

### <a name="event-properties"></a>Vlastnosti události
|Vlastnost  |  Popis |
| - | - |
| ID události | Globálně jedinečný identifikátor pro tuto událost. <br><br> Příklad: <br><ul><li>602d9444-d2cd-49c7-8624-8643e7171297  |
| Typ události | Dopad, který způsobí, že tato událost. <br><br> Hodnoty: <br><ul><li> `Freeze`: hello virtuálního počítače je naplánované toopause několik sekund. Hello procesoru je pozastavená, ale neexistuje žádný vliv na paměti, otevřených souborů nebo připojení k síti. <li>`Reboot`: hello restartování je naplánováno virtuálního počítače (dočasnou paměti dojde ke ztrátě). <li>`Redeploy`: hello virtuálního počítače je naplánované toomove tooanother uzlu (dočasné disky jsou ztraceny). |
| ResourceType | Typ prostředku, který má dopad na tuto událost. <br><br> Hodnoty: <ul><li>`VirtualMachine`|
| Zdroje| Seznam prostředků, které má dopad na tuto událost. Představuje záruku toocontain počítače z maximálně jeden [aktualizace domény](manage-availability.md), ale nemusí obsahovat všechny počítače v hello UD. <br><br> Příklad: <br><ul><li> ["FrontEnd_IN_0", "BackEnd_IN_0"] |
| Stav události. | Stav této události. <br><br> Hodnoty: <ul><li>`Scheduled`: Tato událost je naplánované toostart po dobu hello uvedenou v hello `NotBefore` vlastnost.<li>`Started`: Tato událost byla spuštěna.</ul> Ne `Completed` nebo se někdy poskytuje podobné stav; hello událostí bude vrácen už po dokončení hello událostí.
| Neplatí před| Doba, po jejímž uplynutí může spustit tuto událost. <br><br> Příklad: <br><ul><li> 2016-09-19T18:29:47Z  |

### <a name="event-scheduling"></a>Plánování událostí
Každá událost je naplánováno minimální množství času v budoucnu hello podle typu události. Tentokrát se odrazí v události `NotBefore` vlastnost. 

|Typ události  | Minimální oznámení |
| - | - |
| Zablokování| 15 minut |
| Restartování | 15 minut |
| Opětovné nasazení | 10 minut |

### <a name="starting-an-event"></a>Událost spuštění 

Jakmile jste se naučili nadcházející události a dokončit logika pro řádné vypnutí, můžete schválit hello nevyřízené události tak, že `POST` volání toohello metadata služby s hello `EventId`. To znamená, že ho zmenšit minimální oznámení hello tooAzure čas (Pokud je to možné). 

```
curl -H Metadata:true -X POST -d '{"DocumentIncarnation":"5", "StartRequests": [{"EventId": "f020ba2e-3bc0-4c40-a10b-86575a9eabd5"}]}' http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01
```

> [!NOTE] 
> To v úvahu událost umožňuje tooproceed hello událostí pro všechny `Resources` hello události, ne jenom hello virtuálního počítače, které uznává hello událostí. Proto můžete tooelect vedoucí toocoordinate hello potvrzení, který může být stejně jednoduché jako první počítač hello v hello `Resources` pole.




## <a name="python-sample"></a>Ukázka Pythonu 

Hello následující ukázkové dotazy hello služby metadat pro naplánované události a schválí všechny nevyřízené události.

```python
#!/usr/bin/python

import json
import urllib2
import socket
import sys

metadata_url = "http://169.254.169.254/metadata/scheduledevents?api-version=2017-03-01"
headers = "{Metadata:true}"
this_host = socket.gethostname()

def get_scheduled_events():
   req = urllib2.Request(metadata_url)
   req.add_header('Metadata', 'true')
   resp = urllib2.urlopen(req)
   data = json.loads(resp.read())
   return data

def handle_scheduled_events(data):
    for evt in data['Events']:
        eventid = evt['EventId']
        status = evt['EventStatus']
        resources = evt['Resources']
        eventtype = evt['EventType']
        resourcetype = evt['ResourceType']
        notbefore = evt['NotBefore'].replace(" ","_")
        if this_host in resources:
            print "+ Scheduled Event. This host is scheduled for " + eventype + " not before " + notbefore
            # Add logic for handling events here

def main():
   data = get_scheduled_events()
   handle_scheduled_events(data)
   
if __name__ == '__main__':
  main()
  sys.exit(0)
```

## <a name="next-steps"></a>Další kroky 

- Další informace o rozhraní API dostupná v hello hello [Instance Metadata služby](instance-metadata-service.md).
- Další informace o [plánované údržby pro virtuální počítače s Linuxem v Azure](planned-maintenance.md).
