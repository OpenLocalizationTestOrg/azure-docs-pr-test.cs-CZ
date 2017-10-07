---
title: aaaManage Azure DC/OS cluster s Marathon REST API | Microsoft Docs
description: "Nasazení clusteru Azure Container Service DC/OS tooan kontejnery pomocí rozhraní REST API Marathonu hello."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontejnery, mikroslužby, Mesos, Azure"
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: d926b9b90f5d4eda85a015d9ea0d96fea2c4b566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-container-management-through-hello-marathon-rest-api"></a>Správa kontejnerů DC/OS prostřednictvím hello rozhraní REST API Marathonu
DC/OS poskytuje prostředí pro nasazování a škálování clusterových úloh a zároveň poskytuje abstrakci používaného hardwaru hello. Nad DC/OS je rozhraní, které spravuje plánování a provádění výpočetních úloh. I když jsou k dispozici pro mnoho populárních úloh rozhraní, tento dokument vám pomůže začít vytváření a škálování nasazení kontejnerů pomocí hello Marathon REST API. 

## <a name="prerequisites"></a>Požadavky

Než si projdete tyto příklady, budete potřebovat cluster DC/OS nakonfigurovaný v Azure Container Service. Budete také potřebovat clusteru toothis toohave vzdáleného připojení. Další informace o těchto položek najdete v tématu hello následující články:

* [Nasazení clusteru Azure Container Service](container-service-deployment.md)
* [Připojení clusteru Azure Container Service tooan](../container-service-connect.md)

## <a name="access-hello-dcos-apis"></a>Přístup k rozhraní API DC/OS hello
Jakmile se připojené toohello clusteru Azure Container Service můžete přistupovat hello DC/OS a související rozhraní REST API přes adresu http://localhost: Local-port. Hello příklady v tomto dokumentu předpokládají, že máte k dispozici tunel na portu 80. Například hello Marathon koncové body k dispozici na adrese identifikátory URI počínaje `http://localhost/marathon/v2/`. 

Další informace o hello různých rozhraních API, najdete v dokumentaci Mesosphere hello hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) a [Chronos API](https://mesos.github.io/chronos/docs/api.html)a v dokumentaci Apache pro hello [Mesos Scheduler API ](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Získání informací z DC/OS a Marathonu
Před nasazením clusteru DC/OS toohello kontejnery, zjistěte si určité informace o clusteru DC/OS hello, například názvy hello a stav agentů DC/OS hello. toodo tedy dotaz hello `master/slaves` koncový bod hello REST API DC/OS. Pokud všechno půjde dobře, hello dotaz vrátí seznam agentů DC/OS a několik vlastností, které pro každý.

```bash
curl http://localhost/mesos/master/slaves
```

Nyní pomocí Marathonu hello `/apps` toocheck koncový bod pro aktuální clusteru DC/OS toohello nasazení aplikace. Pokud je to nový cluster, uvidíte prázdné pole aplikací.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Nasazení kontejneru formátovaného Dockerem
Kontejnery formátované Dockerem prostřednictvím hello rozhraní REST API Marathonu nasadíte pomocí souboru JSON, který popisuje hello určený nasazení. Hello následující ukázka nasadí Nginx kontejneru tooa privátní agenta v clusteru hello. 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

toodeploy kontejner formátovaný hello JSON soubor uložte do přístupného umístění. Dále toodeploy hello kontejner, spusťte následující příkaz hello. Zadejte název souboru JSON hello hello (`marathon.json` v tomto příkladu).

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

výstup Hello je podobné toohello následující:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Teď když Marathonu odešlete dotaz pro aplikace, tato nová aplikace se zobrazí ve výstupu hello.

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-hello-container"></a>Dosažení hello kontejneru

Můžete ověřit, že hello Nginx je spuštěn v kontejneru v jednom z hello privátní agenti v clusteru hello. toofind hello hostitele a portu, kde hello kontejneru běží, dotaz Marathon pro hello spuštěných úloh: 

```bash
curl localhost/marathon/v2/tasks
```

Najít hodnotu hello `host` ve výstupu hello (IP adres podobné příliš`10.32.0.x`) a hodnota hello `ports`.


Nyní proveďte SSH terminálu připojení (ne tunelového propojení) toohello správu plně kvalifikovaný název domény clusteru hello. Po připojení, zkontrolujte hello následující požadavek, nahraďte hello správné hodnoty z `host` a `ports`:

```bash
curl http://host:ports
```

Hello výstup Nginx serveru je podobné toohello následující:

![Nginx z kontejneru](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a>Škálování kontejnerů
V nasazení aplikace můžete použít hello Marathon API tooscale out nebo určený počet číslic. V předchozím příkladu hello jste nasadili jednu instanci aplikace. Nyní škálování to zjistit toothree instancí aplikace. toodo tak, vytvořte soubor JSON s použitím hello následující JSON text a uložte ho do přístupného umístění.

```json
{ "instances": 3 }
```

Z tunelového propojení spusťte následující příkaz tooscale se aplikace hello hello.

> [!NOTE]
> Hello identifikátor URI je http://localhost/marathon/v2/apps/ následuje hello ID tooscale aplikace hello. Pokud používáte ukázku Nginx hello, která je k dispozici zde, hello URI by byl http://localhost/marathon/v2/apps/nginx.
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Nakonec dotaz na koncový bod Marathonu hello pro aplikace. Vidíte, že tam jsou nyní tři kontejnery Nginx.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Ekvivalentní příkazy PowerShellu
V systému Windows můžete tyto stejné akce provést pomocí příkazů PowerShellu.

toogather informace o clusteru DC/OS hello, jako jsou názvy agentů a stavu agentů, spusťte následující příkaz hello:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Nasadíte kontejnery formátované Dockerem přes Marathon pomocí souboru JSON, který popisuje hello určený nasazení. Hello následující ukázka nasadí kontejner nginx a sváže hello vazby port 80 hello DC/OS agenta tooport 80 kontejneru hello.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

toodeploy kontejner formátovaný hello JSON soubor uložte do přístupného umístění. Dále toodeploy hello kontejner, spusťte následující příkaz hello. Zadejte soubor JSON toohello cesta hello (`marathon.json` v tomto příkladu).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

V nasazení aplikace můžete použít také hello Marathon API tooscale out nebo škálování. V předchozím příkladu hello jste nasadili jednu instanci aplikace. Nyní škálování to zjistit toothree instancí aplikace. toodo tak, vytvořte soubor JSON s použitím hello následující JSON text a uložte ho do přístupného umístění.

```json
{ "instances": 3 }
```

Spusťte následující příkaz tooscale se aplikace hello hello:

> [!NOTE]
> Hello identifikátor URI je http://localhost/marathon/v2/apps/ následuje hello ID tooscale aplikace hello. Pokud používáte ukázku Nginx hello poskytuje zde, hello identifikátoru URI by http://localhost/marathon/v2/apps/nginx.
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Další kroky
* [Další informace o koncových bodech Mesos HTTP hello](http://mesos.apache.org/documentation/latest/endpoints/)
* [Další informace o hello rozhraní REST API Marathonu](https://mesosphere.github.io/marathon/docs/rest-api.html)

