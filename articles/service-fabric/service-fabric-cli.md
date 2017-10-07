---
title: "aaaGet začít s Azure Service Fabric rozhraní příkazového řádku (sfctl)"
description: "Zjistěte, jak toouse hello Azure Service Fabric rozhraní příkazového řádku. Zjistěte, jak tooconnect tooa clusteru a jak toomanage aplikace."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: f76e8ff65bb38dfb63791da0a23e19b93b337f6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-command-line"></a>Příkazový řádek Azure Service Fabric

Hello Azure Service Fabric rozhraní příkazového řádku (sfctl) je nástroj příkazového řádku pro interakci a správu Azure Service Fabric entity. Sfctl lze použít s clustery systému Windows, nebo Linux. Sfctl běží na libovolné platformě, kde je podporován python.

## <a name="prerequisites"></a>Požadavky

Předchozí tooinstallation, ověřte, zda má prostředí python a nainstalovány nástrojem pip. Další informace, podívejte se na hello [nástrojem pip rychlý start dokumentaci](https://pip.pypa.io/en/latest/quickstart/)a oficiální [python nainstalovat dokumentaci](https://wiki.python.org/moin/BeginnersGuide/Download).

Když jsou podporované obou python 2.7 a 3.6, se doporučuje toouse python 3.6.

## <a name="install"></a>Instalace

Hello Azure Service Fabric rozhraní příkazového řádku (sfctl) je zabalené jako balíček python. tooinstall hello nejnovější verzi spustit:

```bash
pip install sfctl
```

Po instalaci spustit `sfctl -h` tooget informace o dostupné příkazy.

## <a name="cli-syntax"></a>Syntaxe rozhraní příkazového řádku

Příkazy mají vždy předponu `sfctl`. Obecné informace o všech příkazech, které můžete použít, získáte zadáním `sfctl -h`. Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `sfctl <command> -h`.

Postupujte podle příkazy opakovatelných struktura, s cílem hello hello příkaz předchozí příkaz hello nebo akce:

```azurecli
sfctl <object> <action>
```

V tomto příkladu `<object>` hello cíl pro `<action>`.

## <a name="select-a-cluster"></a>Výběr clusteru

Před provedením jakékoli operace, je nutné vybrat tooconnect clusteru k. Například spusťte hello následující tooselect a připojte toohello cluster s názvem hello `testcluster.com`.

> [!WARNING]
> Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

koncový bod clusteru Hello musí začínat řetězcem `http` nebo `https`. Musí zahrnovat port hello hello HTTP brány. Hello port a adresa hello jsou stejné jako hello URL Service Fabric Exploreru.

Pro clustery, které jsou zabezpečené pomocí certifikátu, můžete určit certifikát kódovaný PEM. certifikát Hello lze zadat jako jeden soubor nebo certifikát a pár klíčů.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Další informace najdete v tématu [clusteru zabezpečené Azure Service Fabric připojit tooa](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Základní operace

Informace o připojení ke clusteru se uchovávají napříč více relacemi sfctl. Po vybrání clusteru Service Fabric, můžete spustit všechny příkazy Service Fabric v clusteru hello.

Například tooget hello stav clusteru Service Fabric, použijte následující příkaz hello:

```azurecli
sfctl cluster health
```

příkaz Hello způsobí hello následující výstup:

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

Zde jsou některé návrhy a tipy pro řešení běžných problémů.

### <a name="convert-a-certificate-from-pfx-toopem-format"></a>Převést certifikátu z formátu PFX tooPEM

Hello Service Fabric CLI podporuje klientské certifikáty jako soubory PEM (.pem rozšíření). Pokud používáte soubory PFX ze systému Windows, je nutné převést formát tooPEM tyto certifikáty. tooconvert soubor PEM tooa souboru PFX použijte následující příkaz:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

Další informace najdete v tématu hello [OpenSSL dokumentaci](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Problémy s připojením

Některé operace může generovat hello následující zprávou:

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

Ověřte, že tento hello zadaný koncový bod clusteru je k dispozici a naslouchá. Také ověřte, že hello, které jsou k dispozici v Service Fabric Explorer uživatelského rozhraní, hostitele a portu. koncový bod hello tooupdate, použijte `sfctl cluster select`.

### <a name="detailed-logs"></a>Podrobné protokoly

Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému. Je globální konfiguraci `--debug` příznak, který zvyšuje hello podrobností souborů protokolu.

### <a name="command-help-and-syntax"></a>Nápověda k příkazům a jejich syntaxe

Pro pomoc s konkrétní příkaz nebo skupinu příkazů, použijte hello `-h` příznak:

```azurecli
sfctl application -h
```

Další příklad:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Další kroky

* [Nasazení aplikace s hello příkazového řádku Azure Service Fabric](service-fabric-application-lifecycle-sfctl.md)
* [Začínáme se Service Fabric v Linuxu](service-fabric-get-started-linux.md)
