---
title: aaaAzure Service Fabric Docker Compose Preview
description: "Azure Service Fabric přijímá Docker Compose toomake formát je snazší tooorchestrate stávajících kontejnerů pomocí Service Fabric. Tato podpora je aktuálně ve verzi preview."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: a60d1321fd6ef07b241a98c5ab2b8dfe5d441b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="docker-compose-application-support-in-azure-service-fabric-preview"></a>Podpora aplikací docker Compose v Azure Service Fabric (Preview)

Docker používá hello [docker-compose.yml](https://docs.docker.com/compose) souboru k definování více kontejneru aplikace. toomake ho snadno zákazníky obeznámeni s Docker tooorchestrate existujících kontejneru aplikací v Azure Service Fabric, jsme zahrnuli preview podporu pro Docker Compose nativně hello platformy. Service Fabric může přijmout verze 3 nebo novější z `docker-compose.yml` soubory. 

Protože tato podpora je ve verzi preview, se podporuje jenom podmnožinu vytvářené direktivy. Například upgrady aplikací nejsou podporované. Můžete však vždy odebrat a nasadit aplikace místo jejich aktualizace.

toouse to náhled, vytvořte cluster s verzi 5.7 nebo větší modulu runtime Service Fabric hello prostřednictvím portálu Azure společně s hello odpovídající SDK hello. 

> [!NOTE]
> Tato funkce je ve verzi preview a není podporována v produkčním prostředí.

## <a name="deploy-a-docker-compose-file-on-service-fabric"></a>Nasadit soubor Docker Compose v Service Fabric

Hello následující příkazy vytvořit aplikace Service Fabric (s názvem `fabric:/TestContainerApp` v předchozím příkladu hello), které můžete sledovat a spravovat podobně jako všechny ostatní aplikace Service Fabric. Můžete použít název zadané aplikace hello pro dotazy na stav.

### <a name="use-powershell"></a>Použití prostředí PowerShell

Vytvoření aplikace Service Fabric tvoří z soubor docker-compose.yml tak, že spustíte následující příkaz v prostředí PowerShell hello:

```powershell
New-ServiceFabricComposeApplication -ApplicationName fabric:/TestContainerApp -Compose docker-compose.yml [-RegistryUserName <>] [-RegistryPassword <>] [-PasswordEncrypted]
```

`RegistryUserName`a `RegistryPassword` odkazovat toohello kontejneru registru uživatelské jméno a heslo. Po dokončení aplikace hello, jeho stav můžete zkontrolovat pomocí hello následující příkaz:

```powershell
Get-ServiceFabricComposeApplicationStatus -ApplicationName fabric:/TestContainerApp -GetAllPages
```

toodelete hello vytvářené aplikace pomocí prostředí PowerShell, použijte následující příkaz hello:

```powershell
Remove-ServiceFabricComposeApplication  -ApplicationName fabric:/TestContainerApp
```

### <a name="use-azure-service-fabric-cli-sfctl"></a>Používání Azure Service Fabric rozhraní příkazového řádku (sfctl)

Alternativně můžete použít hello následující služby infrastruktury rozhraní příkazového řádku příkaz:

```azurecli
sfctl compose create --application-id fabric:/TestContainerApp --compose-file docker-compose.yml [ [ --repo-user --repo-pass --encrypted ] | [ --repo-user ] ] [ --timeout ]
```

Po vytvoření aplikace hello, jeho stav můžete zkontrolovat pomocí hello následující příkaz:

```azurecli
sfctl compose status --application-id TestContainerApp [ --timeout ]
```

toodelete hello vytvářené aplikace, použijte následující příkaz hello:

```azurecli
sfctl compose remove  --application-id TestContainerApp [ --timeout ]
```

## <a name="supported-compose-directives"></a>Podporované vytvářené direktivy

Tato verze preview podporuje podmnožinu možnosti konfigurace hello z formátu verze 3 vytvářené hello, včetně následujících primitiv hello:

* Služby > nasazení > repliky
* Služby > nasazení > umístění > omezení
* Služby > nasazení > prostředky > omezení
    * -procesoru-sdílené složky
    * -paměti
    * -paměti-swap
* Služby > příkazy
* Služby > prostředí
* Služby > porty
* Služby > bitové kopie
* Služby > izolace (jenom pro Windows)
* Služby > protokolování > ovladačů
* Služby > protokolování > ovladače > Možnosti
* Svazek & nasazení > svazku

Nastavení clusteru hello pro vynucení omezení prostředků, jak je popsáno v [Service Fabric prostředků zásad správného řízení](service-fabric-resource-governance.md). Všechny ostatní Docker Compose direktivy nejsou podporovány pro tuto verzi preview.

## <a name="servicednsname-computation"></a>Výpočet ServiceDnsName

Pokud je název služby hello, který určíte v souboru vytvářené platný plně kvalifikovaný název domény (to znamená, obsahuje tečku [.]), je název DNS hello registrovaných Service Fabric `<ServiceName>` (včetně hello tečku). Pokud ne, každý segment cesty v názvu aplikace hello se změní na název domény v hello název DNS služby, se první segment cesty hello stal popisek hello doména nejvyšší úrovně.

Například pokud hello zadaný název aplikace je `fabric:/SampleApp/MyComposeApp`, `<ServiceName>.MyComposeApp.SampleApp` by hello registrovaný název DNS.

## <a name="differences-between-compose-instance-definition-and-service-fabric-application-model-type-definition"></a>Rozdíly mezi vytvářené (instance definice) a modelu aplikace Service Fabric (definice typu)

Soubor docker-compose.yml popisuje sadu nasadit kontejnery, včetně jejich vlastnosti a konfigurace.
Například hello soubor může obsahovat proměnné prostředí a porty. Parametry nasazení, jako je například umístění omezení, omezení prostředků a názvy DNS, můžete zadat také v soubor docker-compose.yml hello.

Hello [model aplikace Service Fabric](service-fabric-application-model.md) používá služba typy a typy aplikací, kde může mít velký počet instancí aplikace hello stejného typu. Například můžete mít jedna instance aplikace na zákazníka. Tento model na základě typu podporuje více verzí hello stejného typu aplikace, která je zaregistrovaná u hello runtime.

Například zákazník A může mít aplikaci vytvořit instanci typu 1.0 AppTypeA a zákazník B může mít jiné aplikace vytvořena s hello stejného typu a verzi. Definování typů aplikací hello v hello manifestů aplikace a zadat parametry pro název a nasazení aplikace hello při vytvoření aplikace hello.

I když tento model nabízí flexibilitu, jsme také plánování toosupport model jednodušší, založený na instancích nasazení, kde typy jsou implicitní ze souboru manifestu hello. V tomto modelu každá aplikace získá svůj vlastní nezávislé manifestu. Přidáním podpory pro docker-compose.yml, který je ve formátu nasazení na základě instance jsme náhledu tohoto úsilí.

## <a name="next-steps"></a>Další kroky

* Přečíst na hello [model aplikace Service Fabric](service-fabric-application-model.md)
* [Začínáme s rozhraním příkazového řádku Service Fabric](service-fabric-cli.md)
