---
title: "aaaAzure Instance Metadata služby pro Windows | Microsoft Docs"
description: "Rozhraní rESTful tooget informace o výpočetní, síťové a události nadcházející údržby systému Windows Virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: harijay
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/11/2017
ms.author: harijay
ms.openlocfilehash: a33c26b5e9ed650be639598cdb6895fc19ccb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-instance-metadata-service-for-windows-vms"></a>Instance Metadata služby, které jsou pro virtuální počítače Windows Azure


Hello služby metadat instanci Azure poskytuje informace o spuštěných instancí virtuálního počítače, které se dají použít toomanage a konfigurovat virtuální počítače.
To zahrnuje informace, jako je SKU, konfigurace sítě a události nadcházející údržby. Další informace o jaké typy informací je k dispozici, najdete v části [metadata kategorie](#instance-metadata-data-categories).

Služba Azure Instance metadat je koncový bod REST tooall dostupné virtuální počítače IaaS vytvořené prostřednictvím hello [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). koncový bod Hello je dostupný v dobře známé směrovat IP adresu (`169.254.169.254`), můžete přistupovat pouze z v rámci hello virtuálních počítačů.



> [!IMPORTANT]
> Tato služba je **všeobecně dostupná** v globální oblastech Azure. Je ve verzi Public preview pro státní, Čína a Cloud Azure němčina. Pravidelně obdrží aktualizace tooexpose nové informace o instancí virtuálního počítače. Tato stránka se vztahuje k hello aktuální [kategorie dat](#instance-metadata-data-categories) k dispozici.



## <a name="service-availability"></a>Dostupnost služby
Služba Hello je k dispozici ve všech oblastech všeobecně dostupná globální Azure. Služba Hello je ve verzi public preview v oblastech Government, Čína nebo Německo hello.

Oblasti                                        | Dostupnost?
-----------------------------------------------|-----------------------------------------------
[Všechny všeobecně dostupná globální oblasti Azure](https://azure.microsoft.com/regions/)     | Obecně k dispozici 
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Ve verzi Preview 
[Azure Čína](https://www.azure.cn/)                                                           | Ve verzi Preview
[Azure Německo](https://azure.microsoft.com/overview/clouds/germany/)                    | Ve verzi Preview

Tato tabulka je aktualizována při hello služby k dispozici v ostatních cloudů Azure.

tootry out hello Instance služby metadat, vytvoření virtuálního počítače z [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) nebo hello [portál Azure](http://portal.azure.com) v hello nad oblasti a postupujte podle níže uvedených příkladech hello.

## <a name="usage"></a>Využití

### <a name="versioning"></a>Správa verzí
Hello Instance služby metadat je verzí. Verze jsou povinné a hello aktuální verze je `2017-04-02`.

> [!NOTE] 
> Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako hello api-version. Tento formát se už nepodporuje a bude v budoucí hello nepoužívá.

Jako přidáme novější verze, starší verze se dá dál dostat z důvodu kompatibility Pokud skripty mají závislosti na konkrétní data formátů. Uvědomte si však, že tento hello aktuální preview version(2017-03-01) nemusí být k dispozici, jakmile služba hello je obecně dostupná.

### <a name="using-headers"></a>Používání hlaviček
Když dotazujete hello Instance Metadata služby, je nutné zadat hello záhlaví `Metadata: true` tooensure hello požadavek nebyl přesměrován náhodně.

### <a name="retrieving-metadata"></a>Načítání metadat

Instance metadata jsou k dispozici pro spouštění virtuálních počítačů vytvořena nebo spravovat pomocí [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Přístup k všechny kategorie dat pro instanci virtuálního počítače pomocí hello následující požadavek:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> Všechny dotazy metadat instance rozlišují velká a malá písmena.

### <a name="data-output"></a>Výstup dat
Ve výchozím nastavení, hello Instance Metadata služby vrací data ve formátu JSON (`Content-Type: application/json`). Rozhraní API pro různé však může vrátit data v různých formátech Pokud požadovaný.
Hello následující tabulce je odkazem na jiných formátů dat, které můžou podporovat rozhraní API.

Rozhraní API | Výchozí formát dat | Ostatní formáty
--------|---------------------|--------------
/instance | JSON | Text
/scheduledevents | JSON | None

tooaccess odpovědi jiné než výchozí formát, zadejte požadovaný formát hello jako parametr řetězce dotazu v žádosti o hello. Například:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a>Zabezpečení
koncový bod služby Metadata Instance Hello je dostupné pouze v aplikaci hello spuštěna instance virtuálního počítače na směrovat IP adresu. Kromě toho každá žádost s `X-Forwarded-For` záhlaví byl odmítnut službou hello.
Je také nutné toocontain požadavky `Metadata: true` tooensure hlavičky, která hello skutečné žádosti byl přímo určený a není součástí neúmyslnému přesměrování. 

### <a name="error"></a>Chyba
Pokud je datový prvek nebyl nalezen nebo chybně vytvořený požadavek, vrátí hello Instance Metadata služby standardní chyby protokolu HTTP. Například:

Kód stavu HTTP | Důvod
----------------|-------
200 OK |
400 – Chybný požadavek | Chybí `Metadata: true` záhlaví
404 – Nenalezeno | Existují technologie Hello položka hierarchyinfoguid požadovaný element 
405 – Metoda není povoleno | Pouze `GET` a `POST` jsou podporovány požadavky
429 příliš mnoho požadavků | Hello rozhraní API aktuálně podporuje maximálně 5 dotazů za sekundu
Chyba 500 služby     | Po určité době opakujte

### <a name="examples"></a>Příklady

> [!NOTE] 
> Všechny odpovědi rozhraní API jsou řetězce formátu JSON. Jsou všechny tyto odpovědi příklad pretty vytisknout čitelnější.

#### <a name="retrieving-network-information"></a>Načítání informací o síti

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-04-02"
```

**Odpověď**

> [!NOTE] 
> odpověď Hello je řetězec formátu JSON. Následující příklad odpovědi Hello je pretty vytisknout čitelnější.

```
{
  "interface": [
    {
      "ipv4": {
        "ipAddress": [
          {
            "privateIpAddress": "10.1.0.4",
            "publicIpAddress": "X.X.X.X"
          }
        ],
        "subnet": [
          {
            "address": "10.1.0.0",
            "prefix": "24"
          }
        ]
      },
      "ipv6": {
        "ipAddress": []
      },
      "macAddress": "000D3AF806EC"
    }
  ]
}

```

#### <a name="retrieving-public-ip-address"></a>Načítání veřejnou IP adresu

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network/interface/0/ipv4/ipAddress/0/publicIpAddress?api-version=2017-04-02&format=text"
```

#### <a name="retrieving-all-metadata-for-an-instance"></a>Načítání metadat všechny instance

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

**Odpověď**

> [!NOTE] 
> odpověď Hello je řetězec formátu JSON. Následující příklad odpovědi Hello je pretty vytisknout čitelnější.

```
{
  "compute": {
    "location": "westcentralus",
    "name": "IMDSSample",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "Canonical",
    "sku": "16.04.0-LTS",
    "version": "16.04.201610200",
    "vmId": "5d33a910-a7a0-4443-9f01-6a807801b29b",
    "vmSize": "Standard_A1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.0.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.0.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3AF806EC"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Načítání metadat v systému Windows virtuálního počítače

**Požadavek**

Nejde načíst instance metadata v systému Windows prostřednictvím hello nástroj PowerShell `curl`: 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

Nebo prostřednictvím hello `Invoke-RestMethod` rutiny:
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**Odpověď**

> [!NOTE] 
> odpověď Hello je řetězec formátu JSON. Následující příklad odpovědi Hello je pretty vytisknout čitelnější.

```
{
  "compute": {
    "location": "westus",
    "name": "SQLTest",
    "offer": "SQL2016SP1-WS2016",
    "osType": "Windows",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "MicrosoftSQLServer",
    "sku": "Enterprise",
    "version": "13.0.400110",
    "vmId": "453945c8-3923-4366-b2d3-ea4c80e9b70e",
    "vmSize": "Standard_DS2"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.0.1.4",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.0.1.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": [
            
          ]
        },
        "macAddress": "002248020E1E"
      }
    ]
  }
}
```

## <a name="instance-metadata-data-categories"></a>Kategorie dat instance metadat
Hello následující kategorie dat jsou k dispozici prostřednictvím hello Instance Metadata služby:

Data | Popis
-----|------------
location | Azure oblast hello virtuální počítač spuštěný
jméno | Název hello virtuálních počítačů 
Nabídka | Nabízí informace o hello image virtuálního počítače. Tato hodnota je jenom pro Image nasadit z Galerie obrázků Azure k dispozici.
Vydavatele | Vydavatel hello image virtuálního počítače
SKU | Konkrétní SKU pro hello image virtuálního počítače  
Verze | Verze bitové kopie virtuálních počítačů hello 
osType | Linux nebo Windows 
platformUpdateDomain |  [Aktualizace domény](manage-availability.md) hello virtuální počítač spuštěný v
platformFaultDomain | [Doména selhání](manage-availability.md) hello virtuální počítač spuštěný v
vmId | [Jedinečný identifikátor](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) pro hello virtuálních počítačů
vmSize | [Velikost virtuálního počítače](sizes.md)
IPv4/privateIpAddress | Místní adresa IPv4 hello virtuálních počítačů 
IPv4/publicIpAddress | Veřejnou IPv4 adresu hello virtuálních počítačů
Adresa podsítě / | Adresa podsítě hello virtuálních počítačů
Předpona podsítě / | Předpona podsítě, například 24
IPv6 nebo adresa IP | Místní adresa IPv6 hello virtuálních počítačů
MacAddress | Adresa mac virtuálního počítače 
scheduledevents | Aktuálně ve veřejné verzi Preview najdete [scheduledevents](scheduled-events.md)

## <a name="example-scenarios-for-usage"></a>Příklad scénáře použití  

### <a name="tracking-vm-running-on-azure"></a>Sledování virtuálního počítače v Azure

Jako poskytovatele služeb může vyžadovat tootrack hello počet virtuálních počítačů spuštěných váš software nebo agenty, kteří potřebují tootrack jedinečnosti hello virtuálních počítačů. možnost tooget toobe jedinečné ID pro virtuální počítač, použijte hello `vmId` pole z Instance Metadata služby.

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**Odpověď**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Umístění kontejnery, oddíly dat na základě domény selhání nebo aktualizovat 

Pro určité scénáře, umístění repliky různých datových je důležité. Například [umístění repliky HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) nebo umístění kontejneru prostřednictvím [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) může vyžadovat tooknow hello `platformFaultDomain` a `platformUpdateDomain` hello běží virtuální počítač.
Tato data přímo prostřednictvím hello Instance Metadata služby se můžete dotazovat.

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**Odpověď**

```
0
```

### <a name="getting-more-information-about-hello-vm-during-support-case"></a>Další informace o hello virtuálních počítačů během případu podpory pro získávání 

Jako poskytovatele služeb může se zobrazit žádost o podporu kam chcete tooknow Další informace o hello virtuálních počítačů. S dotazem, tooshare zákazníka hello hello výpočetní metadat poskytuje základní informace pro odborníky v oblasti tooknow hello podpory o hello druh virtuálních počítačů v Azure. 

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**Odpověď**

> [!NOTE] 
> odpověď Hello je řetězec formátu JSON. Následující příklad odpovědi Hello je pretty vytisknout čitelnější.

```
{
  "compute": {
    "location": "CentralUS",
    "name": "IMDSCanary",
    "offer": "RHEL",
    "osType": "Linux",
    "platformFaultDomain": "0",
    "platformUpdateDomain": "0",
    "publisher": "RedHat",
    "sku": "7.2",
    "version": "7.2.20161026",
    "vmId": "5c08b38e-4d57-4c23-ac45-aca61037f084",
    "vmSize": "Standard_DS2"
  }
}
```

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-hello-vm"></a>Příklady volání služby metadat pomocí různých jazyků uvnitř hello virtuálních počítačů 

Jazyk | Příklad 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB
Přejděte Lan   | https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go            
python   | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY
C++      | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp
C#       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH
    

## <a name="faq"></a>Nejčastější dotazy
1. Vyskytla se chyba hello `400 Bad Request, Required metadata header not specified`. Co to znamená?
   * Hello Instance Metadata služby vyžaduje hlavičku hello `Metadata: true` toobe předaná hello požadavku. Předávání tuto hlavičku v volání REST hello umožňuje přístup toohello Instance služby metadat. 
2. Proč není zobrazuje informace o výpočetní pro virtuální počítač?
   * Hello Instance Metadata služby aktuálně podporuje jenom instancích vytvořených pomocí Azure Resource Manageru. V budoucích hello jsme může přidat podporu pro virtuální počítače cloudové služby.
3. Delší dobou vytvořený virtuální počítač prostřednictvím Správce Azure Resource Manager. Proč není najdete informace o metadatech výpočetní?
   * Pro všechny virtuální počítače vytvořené po září 2016, přidejte [značka](../../azure-resource-manager/resource-group-using-tags.md) toostart vidět výpočetní metadat. Pro starší virtuální počítače (vytvořených před září 2016) přidat nebo odebrat rozšíření nebo data toorefresh metadata disky toohello virtuálních počítačů.
4. Proč se zobrazuje chyba hello `500 Internal Server Error`?
   * Opakujte žádost podle exponenciální regrese systému. Pokud hello problém přetrvávat, kontaktujte podporu Azure.
5. Kde mohu sdílet další dotazy nebo připomínky?
   * Odešlete komentáře k http://feedback.azure.com.
7. To funguje pro instanci nastavení škálování virtuálního počítače?
   * Ano je k dispozici pro škálování nastavení instance služby metadat. 
6. Jak získat podporu pro službu hello?
   * tooget podporu služby hello, vytvořit problém podpory na portálu Azure pro hello virtuálního počítače, kde se není možné tooget metadat odpovědi po dlouhou opakování 

   ![Podpora metadat instance](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Další kroky

- Další informace o hello [naplánované události](scheduled-events.md) rozhraní API **ve verzi public preview** poskytované hello Instance Metadata služby.
