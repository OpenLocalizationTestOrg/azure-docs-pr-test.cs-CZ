---
title: "aaaGet začít s Azure Service Fabric a 2.0 rozhraní příkazového řádku Azure"
description: "Zjistěte, jak toouse hello Azure Service Fabric příkazy modulu v Azure CLI verze 2.0. Zjistěte, jak tooconnect tooa clusteru a jak toomanage aplikace."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ddbd0ef503dd3fff61494cc2cfa7c9a2e8d0a9a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-and-azure-cli-20"></a>Azure Service Fabric a Azure CLI 2.0

Hello nástroj příkazového řádku Azure (Azure CLI) verze 2.0 obsahuje příkazy toohelp spravovat Azure Service Fabric clustery. Zjistěte, jak tooget pracovat s Azure CLI a Service Fabric.

## <a name="install-azure-cli-20"></a>Instalace Azure CLI 2.0

Můžete použít příkazy toointeract 2.0 rozhraní příkazového řádku Azure s a správě clusterů Service Fabric. tooget hello nejnovější verzi rozhraní příkazového řádku Azure, postupujte podle hello [Azure CLI 2.0 standardního procesu instalace](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

Další informace najdete v tématu hello [přehled Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Syntaxe Azure CLI

V Azure CLI mají všechny příkazy Service Fabric předponu `az sf`. Obecné informace o hello příkazy, které můžete použít, použijte `az sf -h`. Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `az sf <command> -h`.

Příkazy Service Fabric v Azure CLI používají tento vzorec pojmenování:

```azurecli
az sf <object> <action>
```

`<object>`hello cíl pro `<action>`.

## <a name="select-a-cluster"></a>Výběr clusteru

Před provedením jakékoli operace, je nutné vybrat tooconnect clusteru k. Příklad najdete v tématu hello následující kód. Hello kód se připojí tooan zabezpečená clusteru.

> [!WARNING]
> Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

koncový bod clusteru Hello musí začínat řetězcem `http` nebo `https`. Musí zahrnovat port hello hello HTTP brány. Hello port a adresa hello jsou stejné jako hello URL Service Fabric Exploreru.

Pro clustery zabezpečené pomocí certifikátu můžete použít buď nešifrované soubory .pem, nebo soubory .crt a .key. Například:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Další informace najdete v tématu [clusteru zabezpečené Azure Service Fabric připojit tooa](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Hello `select` příkaz nebude fungovat u všech požadavků před vrátí. tooverify, že jste určili clusteru správně, použijte příkaz jako `az sf cluster health`. Ověřte, že příkaz hello nevrací žádné chyby.

## <a name="basic-operations"></a>Základní operace

Informace o připojení ke clusteru se uchovávají napříč více relacemi Azure CLI. Po vybrání clusteru Service Fabric, můžete spustit všechny příkazy Service Fabric v clusteru hello.

Například tooget hello stav clusteru Service Fabric, použijte následující příkaz hello:

```azurecli
az sf cluster health
```

příkaz Hello způsobí hello následující výstup (za předpokladu, že výstup JSON je zadaný v konfiguraci rozhraní příkazového řádku Azure hello):

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a>Tipy a řešení potíží

Můžete se setkat hello následující informace, které jsou užitečné, pokud narazíte na problémy při použití příkazy Service Fabric v rozhraní příkazového řádku Azure.

### <a name="convert-a-certificate-from-pfx-toopem-format"></a>Převést certifikátu z formátu PFX tooPEM

Azure CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou .pem). Pokud používáte soubory PFX ze systému Windows, je nutné převést formát tooPEM tyto certifikáty. tooconvert soubor PEM tooa souboru PFX použijte hello následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Další informace najdete v tématu hello [OpenSSL dokumentaci](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Problémy s připojením

Některé operace může generovat hello následující zprávou:

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

Ověřte, že tento hello zadaný koncový bod clusteru je k dispozici a naslouchá. Také ověřte, že hello, které jsou k dispozici v Service Fabric Explorer uživatelského rozhraní, hostitele a portu. koncový bod hello tooupdate, použijte `az sf cluster select`.

### <a name="detailed-logs"></a>Podrobné protokoly

Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému. Rozhraní příkazového řádku Azure poskytuje globální konfiguraci `--debug` příznak, který zvyšuje hello podrobností souborů protokolu.

### <a name="command-help-and-syntax"></a>Nápověda k příkazům a jejich syntaxe

Postupujte podle příkazy Service Fabric hello stejné konvence jako rozhraní příkazového řádku Azure. Pro pomoc s konkrétní příkaz nebo skupinu příkazů, použijte hello `-h` příznak:

```azurecli
az sf application -h
```

Tady je další příklad:

```azurecli
az sf application create -h
```
