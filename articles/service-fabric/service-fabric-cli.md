---
title: "Začínáme s Azure Service Fabric CLI (sfctl)"
description: "Naučte se používat Azure Service Fabric CLI. Zjistěte, jak se připojit ke clusteru a jak spravovat aplikace."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: 5ce9adf6c82e3a5521883c5de1e0689d5bf0d94e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-service-fabric-command-line"></a><span data-ttu-id="64966-104">Příkazový řádek Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="64966-104">Azure Service Fabric command line</span></span>

<span data-ttu-id="64966-105">Azure Service Fabric CLI (sfctl) je nástroj příkazového řádku pro práci s entitami Azure Service Fabric a jejich správu.</span><span class="sxs-lookup"><span data-stu-id="64966-105">The Azure Service Fabric CLI (sfctl) is a command-line utility for interacting and managing Azure Service Fabric entities.</span></span> <span data-ttu-id="64966-106">Sfctl lze použít s clustery systému Windows, nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="64966-106">Sfctl can be used with either Windows or Linux clusters.</span></span> <span data-ttu-id="64966-107">Sfctl běží na libovolné platformě, kde je podporován python.</span><span class="sxs-lookup"><span data-stu-id="64966-107">Sfctl runs on any platform where python is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64966-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="64966-108">Prerequisites</span></span>

<span data-ttu-id="64966-109">Před instalací se ujistěte, že ve vašem prostředí je nainstalovaný python a pip.</span><span class="sxs-lookup"><span data-stu-id="64966-109">Prior to installation, make sure your environment has both python and pip installed.</span></span> <span data-ttu-id="64966-110">Další informace najdete v [úvodní dokumentaci nástroje pip](https://pip.pypa.io/en/latest/quickstart/) a oficiální [instalační dokumentaci pro python](https://wiki.python.org/moin/BeginnersGuide/Download).</span><span class="sxs-lookup"><span data-stu-id="64966-110">For more information, take a look at the [pip quickstart documentation](https://pip.pypa.io/en/latest/quickstart/), and official [python install documentation](https://wiki.python.org/moin/BeginnersGuide/Download).</span></span>

<span data-ttu-id="64966-111">I když je podporovaná verze pythonu 2.7 i 3.6, doporučuje se používat python 3.6.</span><span class="sxs-lookup"><span data-stu-id="64966-111">While both python 2.7 and 3.6 are supported, it is recommended to use python 3.6.</span></span>

## <a name="install"></a><span data-ttu-id="64966-112">Instalace</span><span class="sxs-lookup"><span data-stu-id="64966-112">Install</span></span>

<span data-ttu-id="64966-113">Nástroj Azure Service Fabric CLI (sfctl) je zabalený jako balíček pythonu.</span><span class="sxs-lookup"><span data-stu-id="64966-113">The Azure Service Fabric CLI (sfctl) is packaged as a python package.</span></span> <span data-ttu-id="64966-114">Chcete-li nainstalovat nejnovější verzi, spusťte:</span><span class="sxs-lookup"><span data-stu-id="64966-114">To install the latest version run:</span></span>

```bash
pip install sfctl
```

<span data-ttu-id="64966-115">Po instalaci spusťte `sfctl -h`, abyste získali informace o dostupných příkazech.</span><span class="sxs-lookup"><span data-stu-id="64966-115">After installation, run `sfctl -h` to get information about available commands.</span></span>

## <a name="cli-syntax"></a><span data-ttu-id="64966-116">Syntaxe rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="64966-116">CLI syntax</span></span>

<span data-ttu-id="64966-117">Příkazy mají vždy předponu `sfctl`.</span><span class="sxs-lookup"><span data-stu-id="64966-117">Commands are prefixed always with `sfctl`.</span></span> <span data-ttu-id="64966-118">Obecné informace o všech příkazech, které můžete použít, získáte zadáním `sfctl -h`.</span><span class="sxs-lookup"><span data-stu-id="64966-118">For general information about all commands you can use, use `sfctl -h`.</span></span> <span data-ttu-id="64966-119">Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `sfctl <command> -h`.</span><span class="sxs-lookup"><span data-stu-id="64966-119">For help with a single command, use `sfctl <command> -h`.</span></span>

<span data-ttu-id="64966-120">Příkazy dodržují opakovatelnou strukturu, kdy cíl příkazu předchází operaci nebo akci:</span><span class="sxs-lookup"><span data-stu-id="64966-120">Commands follow a repeatable structure, with the target of the command preceding the verb or action:</span></span>

```azurecli
sfctl <object> <action>
```

<span data-ttu-id="64966-121">V tomto příkladu je `<object>` cílem pro `<action>`.</span><span class="sxs-lookup"><span data-stu-id="64966-121">In this example, `<object>` is the target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="64966-122">Výběr clusteru</span><span class="sxs-lookup"><span data-stu-id="64966-122">Select a cluster</span></span>

<span data-ttu-id="64966-123">Před provedením jakékoli operace musíte vybrat cluster, ke kterému se připojíte.</span><span class="sxs-lookup"><span data-stu-id="64966-123">Before you perform any operations, you must select a cluster to connect to.</span></span> <span data-ttu-id="64966-124">Například spuštěním následujícího příkazu vyberete cluster s názvem `testcluster.com` a připojíte se k němu.</span><span class="sxs-lookup"><span data-stu-id="64966-124">For example, run the following to select and connect to the cluster with the name `testcluster.com`.</span></span>

> [!WARNING]
> <span data-ttu-id="64966-125">Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="64966-125">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="64966-126">Koncový bod clusteru musí mít předponu `http` nebo `https`.</span><span class="sxs-lookup"><span data-stu-id="64966-126">The cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="64966-127">Musí zahrnovat port pro bránu HTTP.</span><span class="sxs-lookup"><span data-stu-id="64966-127">It must include the port for the HTTP gateway.</span></span> <span data-ttu-id="64966-128">Tento port a adresa jsou stejné jako adresa URL nástroje Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="64966-128">The port and address are the same as the Service Fabric Explorer URL.</span></span>

<span data-ttu-id="64966-129">Pro clustery, které jsou zabezpečené pomocí certifikátu, můžete určit certifikát kódovaný PEM.</span><span class="sxs-lookup"><span data-stu-id="64966-129">For clusters that are secured with a certificate, you can specify a PEM encoded certificate.</span></span> <span data-ttu-id="64966-130">Certifikát lze zadat jako jeden soubor, nebo jako dvojici certifikátu a klíče.</span><span class="sxs-lookup"><span data-stu-id="64966-130">The certificate can be specified as a single file or cert and key pair.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="64966-131">Další informace najdete v tématu [Připojení k zabezpečenému clusteru Azure Service Fabric](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="64966-131">For more information, see [Connect to a secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="basic-operations"></a><span data-ttu-id="64966-132">Základní operace</span><span class="sxs-lookup"><span data-stu-id="64966-132">Basic operations</span></span>

<span data-ttu-id="64966-133">Informace o připojení ke clusteru se uchovávají napříč více relacemi sfctl.</span><span class="sxs-lookup"><span data-stu-id="64966-133">Cluster connection information persists across multiple sfctl sessions.</span></span> <span data-ttu-id="64966-134">Po výběru clusteru Service Fabric na něm můžete spouštět jakékoli příkazy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="64966-134">After you select a Service Fabric cluster, you can run any Service Fabric command on the cluster.</span></span>

<span data-ttu-id="64966-135">Pokud například chcete získat stav clusteru Service Fabric, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="64966-135">For example, to get the Service Fabric cluster health state, use the following command:</span></span>

```azurecli
sfctl cluster health
```

<span data-ttu-id="64966-136">Příkaz vrátí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="64966-136">The command results in the following output:</span></span>

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

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="64966-137">Tipy a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="64966-137">Tips and troubleshooting</span></span>

<span data-ttu-id="64966-138">Zde jsou některé návrhy a tipy pro řešení běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="64966-138">Some suggestions and tips for solving common issues.</span></span>

### <a name="convert-a-certificate-from-pfx-to-pem-format"></a><span data-ttu-id="64966-139">Převod certifikátu z formátu PFX na PEM</span><span class="sxs-lookup"><span data-stu-id="64966-139">Convert a certificate from PFX to PEM format</span></span>

<span data-ttu-id="64966-140">Service Fabric CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou .pem).</span><span class="sxs-lookup"><span data-stu-id="64966-140">The Service Fabric CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="64966-141">Pokud používáte soubory PFX ze systému Windows, musíte tyto certifikáty převést na formát PEM.</span><span class="sxs-lookup"><span data-stu-id="64966-141">If you use PFX files from Windows, you must convert those certificates to PEM format.</span></span> <span data-ttu-id="64966-142">K převodu souboru PFX na soubor PEM použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="64966-142">To convert a PFX file to a PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="64966-143">Další informace najdete v [dokumentace k OpenSSL](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="64966-143">For more information, see the [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="64966-144">Problémy s připojením</span><span class="sxs-lookup"><span data-stu-id="64966-144">Connection issues</span></span>

<span data-ttu-id="64966-145">Některé operace můžou generovat následující zprávu:</span><span class="sxs-lookup"><span data-stu-id="64966-145">Some operations might generate the following message:</span></span>

`Failed to establish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="64966-146">Ověřte, že zadaný koncový bod clusteru je dostupný a naslouchá.</span><span class="sxs-lookup"><span data-stu-id="64966-146">Verify that the specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="64966-147">Ověřte také, že je na daném hostiteli a portu dostupné uživatelské rozhraní Service Fabric Explorer.</span><span class="sxs-lookup"><span data-stu-id="64966-147">Also, verify that the Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="64966-148">Pokud chcete aktualizovat koncový bod, použijte příkaz `sfctl cluster select`.</span><span class="sxs-lookup"><span data-stu-id="64966-148">To update the endpoint, use `sfctl cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="64966-149">Podrobné protokoly</span><span class="sxs-lookup"><span data-stu-id="64966-149">Detailed logs</span></span>

<span data-ttu-id="64966-150">Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému.</span><span class="sxs-lookup"><span data-stu-id="64966-150">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="64966-151">Existuje globální příznak `--debug`, kterým se zvyšuje úroveň podrobností souborů protokolů.</span><span class="sxs-lookup"><span data-stu-id="64966-151">There is a global `--debug` flag that increases the verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="64966-152">Nápověda k příkazům a jejich syntaxe</span><span class="sxs-lookup"><span data-stu-id="64966-152">Command help and syntax</span></span>

<span data-ttu-id="64966-153">Pokud chcete získat nápovědu ke konkrétnímu příkazu nebo skupině příkazů, použijte příznak `-h`:</span><span class="sxs-lookup"><span data-stu-id="64966-153">For help with a specific command or a group of commands, use the `-h` flag:</span></span>

```azurecli
sfctl application -h
```

<span data-ttu-id="64966-154">Další příklad:</span><span class="sxs-lookup"><span data-stu-id="64966-154">Another example:</span></span>

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a><span data-ttu-id="64966-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="64966-155">Next steps</span></span>

* [<span data-ttu-id="64966-156">Nasazení aplikace pomocí Azure Service Fabric CLI</span><span class="sxs-lookup"><span data-stu-id="64966-156">Deploy an application with the Azure Service Fabric CLI</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="64966-157">Začínáme se Service Fabric v Linuxu</span><span class="sxs-lookup"><span data-stu-id="64966-157">Get started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
