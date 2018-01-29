---
title: "Služba Azure Instance metadat | Microsoft Docs"
description: "Rozhraní rESTful získat informace o výpočetní, síťové a nadcházející údržbě události Linux Virtuálního počítače."
services: virtual-machines-linux
documentationcenter: 
author: harijayms
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 10/10/2017
ms.author: harijayms
ms.openlocfilehash: 40b684fe5681123f3c32d3984b2725f97b427f1b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="azure-instance-metadata-service"></a>Služba Azure Instance metadat


Služba Azure Instance metadat poskytuje informace o spuštěných instancí virtuálního počítače, které lze použít ke správě nebo konfiguraci virtuálních počítačů.
To zahrnuje informace, jako je SKU, konfigurace sítě a události nadcházející údržby. Další informace o jaké typy informací je k dispozici, najdete v části [metadata kategorie](#instance-metadata-data-categories).

Služba Metadata Instance Azure a je přístupný pro všechny virtuální počítače IaaS vytvořené prostřednictvím koncový bod REST [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Koncový bod je k dispozici na dobře známé směrovat IP adresu (`169.254.169.254`), můžete přistupovat pouze z virtuálního počítače.

> [!IMPORTANT]
> Tato služba je **všeobecně dostupná** ve všech oblastech Azure.  Pravidelně obdrží aktualizace ke zveřejnění nové informace o instancí virtuálního počítače. Tato stránka zobrazuje aktuální [kategorie dat](#instance-metadata-data-categories) k dispozici.

## <a name="service-availability"></a>Dostupnost služeb
Služba je k dispozici ve všech obecně k dispozici všechny oblasti Azure. Ne všechny verze rozhraní API může být k dispozici ve všech oblastech Azure.

Oblasti                                        | Dostupnost?                                 | Podporované verze
-----------------------------------------------|-----------------------------------------------|-----------------
[Všechny všeobecně dostupná globální oblasti Azure](https://azure.microsoft.com/regions/)     | Obecně k dispozici   | 2017-04-02, 2017-08-01
[Azure Government](https://azure.microsoft.com/overview/clouds/government/)              | Obecně k dispozici | 2017-04-02
[Azure Čína](https://www.azure.cn/)                                                           | Obecně k dispozici | 2017-04-02
[Azure Německo](https://azure.microsoft.com/overview/clouds/germany/)                    | Obecně k dispozici | 2017-04-02

Tato tabulka je aktualizován, jakmile jsou aktualizace služby a nebo nové podporované verze jsou k dispozici

Můžete vyzkoušet na služby metadat Instance, vytvoření virtuálního počítače z [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/) nebo [portál Azure](http://portal.azure.com) v oblastech výše a postupujte podle níže uvedených příkladech.

## <a name="usage"></a>Využití

### <a name="versioning"></a>Správa verzí
Služba Instance metadat je verzí. Verze jsou povinné a je aktuální verze na globální Azure `2017-08-01`. Aktuální podporované verze jsou (2017-04-02, 2017-08-01)

> [!NOTE] 
> Předchozí verze preview naplánované událostí podporovaných {nejnovější} jako verze rozhraní api. Tento formát se už nepodporuje a bude v budoucnu zastaralá.

Jako přidáme novější verze, starší verze se dá dál dostat z důvodu kompatibility Pokud skripty mají závislosti na konkrétní data formátů. Všimněte si však, že předchozí verze preview version(2017-03-01) nemusí být k dispozici, jakmile služba všeobecně dostupná.

### <a name="using-headers"></a>Používání hlaviček
Když dotazujete Metadata Instance služby, je nutné zadat hlavičku `Metadata: true` zajistit požadavek nebyl přesměrován náhodně.

### <a name="retrieving-metadata"></a>Načítání metadat

Instance metadata jsou k dispozici pro spouštění virtuálních počítačů vytvořena nebo spravovat pomocí [Azure Resource Manager](https://docs.microsoft.com/rest/api/resources/). Všechny kategorie dat pro instanci virtuálního počítače pomocí následující žádosti o přístup:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02"
```

> [!NOTE] 
> Všechny dotazy metadat instance rozlišují velká a malá písmena.

### <a name="data-output"></a>Výstup dat
Ve výchozím nastavení, Instance služby metadat vrací data ve formátu JSON (`Content-Type: application/json`). Ale jiné rozhraní API vrátit data v různých formátech, pokud požadovaný.
V následující tabulce je odkazem na jiných formátů dat, které můžou podporovat rozhraní API.

Rozhraní API | Výchozí formát dat | Ostatní formáty
--------|---------------------|--------------
/instance | json | Text
/scheduledevents | json | Žádné

Pro přístup k odpovědi jiné než výchozí formát, zadejte požadovaný formát jako parametr řetězce dotazu v žádosti. Příklad:

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-04-02&format=text"
```

### <a name="security"></a>Zabezpečení
Koncový bod služby Metadata Instance je dostupné pouze v aplikaci spuštěnou instanci virtuálního počítače na směrovat IP adresu. Kromě toho každá žádost s `X-Forwarded-For` záhlaví byl odmítnut službou.
Je také nutné požadavky tak, aby obsahovala `Metadata: true` záhlaví zajistit, že skutečné žádost byla přímo určený a není součástí neúmyslnému přesměrování. 

### <a name="error"></a>Chyba
Pokud je datový prvek nebyl nalezen nebo chybně vytvořený požadavek, Instance služby metadat vrátí standardní chyby protokolu HTTP. Příklad:

Kód stavu HTTP | Důvod
----------------|-------
200 OK |
400 – Chybný požadavek | Chybí `Metadata: true` záhlaví
404 – Nenalezeno | Požadovaný element neexistuje. 
405 – Metoda není povoleno | Pouze `GET` a `POST` jsou podporovány požadavky
429 příliš mnoho požadavků | Rozhraní API aktuálně podporuje maximálně 5 dotazů za sekundu
Chyba 500 služby     | Po určité době opakujte

### <a name="examples"></a>Příklady

> [!NOTE] 
> Všechny odpovědi rozhraní API jsou řetězce formátu JSON. Jsou všechny tyto odpovědi příklad pretty vytisknout čitelnější.

#### <a name="retrieving-network-information"></a>Načítání informací o síti

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/network?api-version=2017-08-01"
```

**Odpověď**

> [!NOTE] 
> Odpověď je řetězec formátu JSON. Následující příklad odpověď je pretty vytisknout čitelnější.

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
curl -H Metadata:true "http://169.254.169.254/metadata/instance?api-version=2017-08-01"
```

**Odpověď**

> [!NOTE] 
> Odpověď je řetězec formátu JSON. Následující příklad odpověď je pretty vytisknout čitelnější.

```
{
  "compute": {
    "location": "westus",
    "name": "avset2",
    "offer": "UbuntuServer",
    "osType": "Linux",
    "placementGroupId": "",
    "platformFaultDomain": "1",
    "platformUpdateDomain": "1",
    "publisher": "Canonical",
    "resourceGroupName": "myrg",
    "sku": "16.04-LTS",
    "subscriptionId": "xxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "tags": "",
    "version": "16.04.201708030",
    "vmId": "13f56399-bd52-4150-9748-7190aae1ff21",
    "vmSize": "Standard_D1"
  },
  "network": {
    "interface": [
      {
        "ipv4": {
          "ipAddress": [
            {
              "privateIpAddress": "10.1.2.5",
              "publicIpAddress": "X.X.X.X"
            }
          ],
          "subnet": [
            {
              "address": "10.1.2.0",
              "prefix": "24"
            }
          ]
        },
        "ipv6": {
          "ipAddress": []
        },
        "macAddress": "000D3A36DDED"
      }
    ]
  }
}
```

#### <a name="retrieving-metadata-in-windows-virtual-machine"></a>Načítání metadat virtuálního počítače ve Windows

**Požadavek**

Nejde načíst instance metadata v systému Windows pomocí nástroje PowerShell `curl`: 

```
curl -H @{'Metadata'='true'} http://169.254.169.254/metadata/instance?api-version=2017-04-02 | select -ExpandProperty Content
```

Nebo pomocí `Invoke-RestMethod` rutiny:
    
```
Invoke-RestMethod -Headers @{"Metadata"="true"} -URI http://169.254.169.254/metadata/instance?api-version=2017-04-02 -Method get 
```

**Odpověď**

> [!NOTE] 
> Odpověď je řetězec formátu JSON. Následující příklad odpověď je pretty vytisknout čitelnější.

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
Následující kategorie dat jsou k dispozici prostřednictvím služby Instance metadat:

Data | Popis | Verze zavedená 
-----|-------------|-----------------------
location | Oblast Azure virtuální počítač běží v | 2017-04-02 
jméno | Název virtuálního počítače | 2017-04-02
nabídka | Nabízí informace o image virtuálního počítače. Tato hodnota je jenom pro Image nasadit z Galerie obrázků Azure k dispozici. | 2017-04-02
Vydavatele | Vydavatel image virtuálního počítače | 2017-04-02
SKU | Konkrétní SKU pro bitovou kopii virtuálního počítače | 2017-04-02
verze | Verze bitové kopie virtuálního počítače | 2017-04-02
osType | Linux nebo Windows | 2017-04-02
platformUpdateDomain |  [Aktualizace domény](manage-availability.md) je virtuální počítač spuštěný | 2017-04-02
platformFaultDomain | [Doména selhání](manage-availability.md) je virtuální počítač spuštěný | 2017-04-02
vmId | [Jedinečný identifikátor](https://azure.microsoft.com/blog/accessing-and-using-azure-vm-unique-id/) pro virtuální počítač. | 2017-04-02
vmSize | [Velikost virtuálního počítače](sizes.md) | 2017-04-02
subscriptionId | Předplatné Azure pro virtuální počítač | 2017-08-01
tags | [Značky](../../azure-resource-manager/resource-group-using-tags.md) pro virtuální počítač  | 2017-08-01
Název skupiny prostředků | [Skupina prostředků](../../azure-resource-manager/resource-group-overview.md) pro virtuální počítač | 2017-08-01
placementGroupId | [Umístění skupiny](../../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) váš virtuální počítač měřítka nastavit | 2017-08-01
IPv4/privateIpAddress | Místní adresu IPv4 virtuálního počítače | 2017-04-02
IPv4/publicIpAddress | Veřejnou IPv4 adresu virtuálního počítače | 2017-04-02
Adresa podsítě / | Adresa podsítě virtuálního počítače | 2017-04-02 
Předpona podsítě / | Předpona podsítě, například 24 | 2017-04-02 
IPv6 nebo adresa IP | Místní adresu IPv6 virtuálního počítače | 2017-04-02 
MacAddress | Adresa mac virtuálního počítače | 2017-04-02 
scheduledevents | Aktuálně ve veřejné verzi Preview najdete [scheduledevents](scheduled-events.md) | 2017-03-01

## <a name="example-scenarios-for-usage"></a>Příklad scénáře použití  

### <a name="tracking-vm-running-on-azure"></a>Sledování virtuálního počítače v Azure

Jako poskytovatele služeb může vyžadovat sledovat počet virtuálních počítačů spuštěných váš software nebo agenty, kteří potřebují ke sledování jedinečnosti virtuálního počítače. Abyste mohli získat jedinečné ID pro virtuální počítač, použijte `vmId` pole z Instance Metadata služby.

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/vmId?api-version=2017-04-02&format=text"
```

**Odpověď**

```
5c08b38e-4d57-4c23-ac45-aca61037f084
```

### <a name="placement-of-containers-data-partitions-based-faultupdate-domain"></a>Umístění kontejnery, oddíly dat na základě domény selhání nebo aktualizovat 

Pro určité scénáře, umístění repliky různých datových je důležité. Například [umístění repliky HDFS](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Replica_Placement:_The_First_Baby_Steps) nebo umístění kontejneru prostřednictvím [orchestrator](https://kubernetes.io/docs/user-guide/node-selection/) může vyžadovat vědět `platformFaultDomain` a `platformUpdateDomain` virtuální počítač běží na.
Tato data přímo přes službu Metadata Instance se můžete dotazovat.

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute/platformFaultDomain?api-version=2017-04-02&format=text" 
```

**Odpověď**

```
0
```

### <a name="getting-more-information-about-the-vm-during-support-case"></a>Další informace o virtuálním počítači během případu podpory pro získávání 

Jako poskytovatele služeb může získat volání podpory kde chcete vědět, další informace o virtuálním počítači. S dotazem, zákazník sdílet metadata výpočetní poskytuje základní informace pro odborníky v oblasti vědět o druh virtuálního počítače na platformě Azure podporu. 

**Požadavek**

```
curl -H Metadata:true "http://169.254.169.254/metadata/instance/compute?api-version=2017-04-02"
```

**Odpověď**

> [!NOTE] 
> Odpověď je řetězec formátu JSON. Následující příklad odpověď je pretty vytisknout čitelnější.

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

### <a name="examples-of-calling-metadata-service-using-different-languages-inside-the-vm"></a>Příklady volání služby metadat pomocí různých jazyků ve virtuálním počítači 

Jazyk | Příklad: 
---------|----------------
Ruby     | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.RB
Přejít  | https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.go            
Python   | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PY
C++      | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample-Windows.cpp
C#       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.cs
JavaScript | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.js
PowerShell | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.ps1
Bash       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.SH
Perl       | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.PL
Java       | https://github.com/Microsoft/azureimds/BLOB/Master/imdssample.Java
Visual Basic | https://github.com/Microsoft/azureimds/BLOB/Master/IMDSSample.vb
    

## <a name="faq"></a>Nejčastější dotazy
1. Vyskytla se chyba `400 Bad Request, Required metadata header not specified`. Co to znamená?
   * Instance Metadata služba vyžaduje, aby záhlaví `Metadata: true` předávané v požadavku. Předávání tuto hlavičku ve volání REST umožňuje přístup ke službě Instance metadat. 
2. Proč není zobrazuje informace o výpočetní pro virtuální počítač?
   * Instance služby Metadata aktuálně podporuje jenom instancích vytvořených pomocí Azure Resource Manageru. V budoucnu jsme může přidat podporu pro virtuální počítače cloudové služby.
3. Delší dobou vytvořený virtuální počítač prostřednictvím Správce Azure Resource Manager. Proč není vidět informace metadat výpočetní?
   * Pro všechny virtuální počítače vytvořené po září 2016, přidejte [značky](../../azure-resource-manager/resource-group-using-tags.md) zahájíte zobrazuje výpočetní metadat. Pro starší virtuální počítače (vytvořených před září 2016) přidat nebo odebrat rozšíření nebo data disky na virtuální počítač aktualizujte metadata.
4. Nejsou zobrazeny všechna data vyplněný pro novou verzi 2017-08-01
   * Pro všechny virtuální počítače vytvořené po září 2016, přidejte [značky](../../azure-resource-manager/resource-group-using-tags.md) zahájíte zobrazuje výpočetní metadat. Pro starší virtuální počítače (vytvořených před září 2016) přidat nebo odebrat rozšíření nebo data disky na virtuální počítač aktualizujte metadata.
5. Proč se zobrazuje chyba `500 Internal Server Error`?
   * Exponenciální regrese systému podle žádost zadat znova. Pokud potíže potrvají, kontaktujte podporu Azure.
6. Kde mohu sdílet další dotazy nebo připomínky?
   * Odešlete komentáře k http://feedback.azure.com.
7. To funguje pro instanci nastavení škálování virtuálního počítače?
   * Ano je k dispozici pro škálování nastavení instance služby metadat. 
8. Jak získat podporu pro službu?
   * Získat podporu pro službu, vytvořte žádost o podporu na portálu Azure pro virtuální počítač, kde není možné získat metadata odpověď po dlouhou opakování 

   ![Podpora metadat instance](./media/instance-metadata-service/InstanceMetadata-support.png)
    
## <a name="next-steps"></a>Další postup

- Další informace o [naplánované události](scheduled-events.md) rozhraní API **ve verzi public preview** poskytovaný službou Instance metadat.
