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
# <a name="azure-service-fabric-command-line"></a><span data-ttu-id="ce42c-104">Příkazový řádek Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ce42c-104">Azure Service Fabric command line</span></span>

<span data-ttu-id="ce42c-105">Hello Azure Service Fabric rozhraní příkazového řádku (sfctl) je nástroj příkazového řádku pro interakci a správu Azure Service Fabric entity.</span><span class="sxs-lookup"><span data-stu-id="ce42c-105">hello Azure Service Fabric CLI (sfctl) is a command-line utility for interacting and managing Azure Service Fabric entities.</span></span> <span data-ttu-id="ce42c-106">Sfctl lze použít s clustery systému Windows, nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="ce42c-106">Sfctl can be used with either Windows or Linux clusters.</span></span> <span data-ttu-id="ce42c-107">Sfctl běží na libovolné platformě, kde je podporován python.</span><span class="sxs-lookup"><span data-stu-id="ce42c-107">Sfctl runs on any platform where python is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce42c-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ce42c-108">Prerequisites</span></span>

<span data-ttu-id="ce42c-109">Předchozí tooinstallation, ověřte, zda má prostředí python a nainstalovány nástrojem pip.</span><span class="sxs-lookup"><span data-stu-id="ce42c-109">Prior tooinstallation, make sure your environment has both python and pip installed.</span></span> <span data-ttu-id="ce42c-110">Další informace, podívejte se na hello [nástrojem pip rychlý start dokumentaci](https://pip.pypa.io/en/latest/quickstart/)a oficiální [python nainstalovat dokumentaci](https://wiki.python.org/moin/BeginnersGuide/Download).</span><span class="sxs-lookup"><span data-stu-id="ce42c-110">For more information, take a look at hello [pip quickstart documentation](https://pip.pypa.io/en/latest/quickstart/), and official [python install documentation](https://wiki.python.org/moin/BeginnersGuide/Download).</span></span>

<span data-ttu-id="ce42c-111">Když jsou podporované obou python 2.7 a 3.6, se doporučuje toouse python 3.6.</span><span class="sxs-lookup"><span data-stu-id="ce42c-111">While both python 2.7 and 3.6 are supported, it is recommended toouse python 3.6.</span></span>

## <a name="install"></a><span data-ttu-id="ce42c-112">Instalace</span><span class="sxs-lookup"><span data-stu-id="ce42c-112">Install</span></span>

<span data-ttu-id="ce42c-113">Hello Azure Service Fabric rozhraní příkazového řádku (sfctl) je zabalené jako balíček python.</span><span class="sxs-lookup"><span data-stu-id="ce42c-113">hello Azure Service Fabric CLI (sfctl) is packaged as a python package.</span></span> <span data-ttu-id="ce42c-114">tooinstall hello nejnovější verzi spustit:</span><span class="sxs-lookup"><span data-stu-id="ce42c-114">tooinstall hello latest version run:</span></span>

```bash
pip install sfctl
```

<span data-ttu-id="ce42c-115">Po instalaci spustit `sfctl -h` tooget informace o dostupné příkazy.</span><span class="sxs-lookup"><span data-stu-id="ce42c-115">After installation, run `sfctl -h` tooget information about available commands.</span></span>

## <a name="cli-syntax"></a><span data-ttu-id="ce42c-116">Syntaxe rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="ce42c-116">CLI syntax</span></span>

<span data-ttu-id="ce42c-117">Příkazy mají vždy předponu `sfctl`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-117">Commands are prefixed always with `sfctl`.</span></span> <span data-ttu-id="ce42c-118">Obecné informace o všech příkazech, které můžete použít, získáte zadáním `sfctl -h`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-118">For general information about all commands you can use, use `sfctl -h`.</span></span> <span data-ttu-id="ce42c-119">Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `sfctl <command> -h`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-119">For help with a single command, use `sfctl <command> -h`.</span></span>

<span data-ttu-id="ce42c-120">Postupujte podle příkazy opakovatelných struktura, s cílem hello hello příkaz předchozí příkaz hello nebo akce:</span><span class="sxs-lookup"><span data-stu-id="ce42c-120">Commands follow a repeatable structure, with hello target of hello command preceding hello verb or action:</span></span>

```azurecli
sfctl <object> <action>
```

<span data-ttu-id="ce42c-121">V tomto příkladu `<object>` hello cíl pro `<action>`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-121">In this example, `<object>` is hello target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="ce42c-122">Výběr clusteru</span><span class="sxs-lookup"><span data-stu-id="ce42c-122">Select a cluster</span></span>

<span data-ttu-id="ce42c-123">Před provedením jakékoli operace, je nutné vybrat tooconnect clusteru k.</span><span class="sxs-lookup"><span data-stu-id="ce42c-123">Before you perform any operations, you must select a cluster tooconnect to.</span></span> <span data-ttu-id="ce42c-124">Například spusťte hello následující tooselect a připojte toohello cluster s názvem hello `testcluster.com`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-124">For example, run hello following tooselect and connect toohello cluster with hello name `testcluster.com`.</span></span>

> [!WARNING]
> <span data-ttu-id="ce42c-125">Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="ce42c-125">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="ce42c-126">koncový bod clusteru Hello musí začínat řetězcem `http` nebo `https`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-126">hello cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="ce42c-127">Musí zahrnovat port hello hello HTTP brány.</span><span class="sxs-lookup"><span data-stu-id="ce42c-127">It must include hello port for hello HTTP gateway.</span></span> <span data-ttu-id="ce42c-128">Hello port a adresa hello jsou stejné jako hello URL Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="ce42c-128">hello port and address are hello same as hello Service Fabric Explorer URL.</span></span>

<span data-ttu-id="ce42c-129">Pro clustery, které jsou zabezpečené pomocí certifikátu, můžete určit certifikát kódovaný PEM.</span><span class="sxs-lookup"><span data-stu-id="ce42c-129">For clusters that are secured with a certificate, you can specify a PEM encoded certificate.</span></span> <span data-ttu-id="ce42c-130">certifikát Hello lze zadat jako jeden soubor nebo certifikát a pár klíčů.</span><span class="sxs-lookup"><span data-stu-id="ce42c-130">hello certificate can be specified as a single file or cert and key pair.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="ce42c-131">Další informace najdete v tématu [clusteru zabezpečené Azure Service Fabric připojit tooa](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="ce42c-131">For more information, see [Connect tooa secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="basic-operations"></a><span data-ttu-id="ce42c-132">Základní operace</span><span class="sxs-lookup"><span data-stu-id="ce42c-132">Basic operations</span></span>

<span data-ttu-id="ce42c-133">Informace o připojení ke clusteru se uchovávají napříč více relacemi sfctl.</span><span class="sxs-lookup"><span data-stu-id="ce42c-133">Cluster connection information persists across multiple sfctl sessions.</span></span> <span data-ttu-id="ce42c-134">Po vybrání clusteru Service Fabric, můžete spustit všechny příkazy Service Fabric v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="ce42c-134">After you select a Service Fabric cluster, you can run any Service Fabric command on hello cluster.</span></span>

<span data-ttu-id="ce42c-135">Například tooget hello stav clusteru Service Fabric, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ce42c-135">For example, tooget hello Service Fabric cluster health state, use hello following command:</span></span>

```azurecli
sfctl cluster health
```

<span data-ttu-id="ce42c-136">příkaz Hello způsobí hello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="ce42c-136">hello command results in hello following output:</span></span>

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

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="ce42c-137">Tipy a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ce42c-137">Tips and troubleshooting</span></span>

<span data-ttu-id="ce42c-138">Zde jsou některé návrhy a tipy pro řešení běžných problémů.</span><span class="sxs-lookup"><span data-stu-id="ce42c-138">Some suggestions and tips for solving common issues.</span></span>

### <a name="convert-a-certificate-from-pfx-toopem-format"></a><span data-ttu-id="ce42c-139">Převést certifikátu z formátu PFX tooPEM</span><span class="sxs-lookup"><span data-stu-id="ce42c-139">Convert a certificate from PFX tooPEM format</span></span>

<span data-ttu-id="ce42c-140">Hello Service Fabric CLI podporuje klientské certifikáty jako soubory PEM (.pem rozšíření).</span><span class="sxs-lookup"><span data-stu-id="ce42c-140">hello Service Fabric CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="ce42c-141">Pokud používáte soubory PFX ze systému Windows, je nutné převést formát tooPEM tyto certifikáty.</span><span class="sxs-lookup"><span data-stu-id="ce42c-141">If you use PFX files from Windows, you must convert those certificates tooPEM format.</span></span> <span data-ttu-id="ce42c-142">tooconvert soubor PEM tooa souboru PFX použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ce42c-142">tooconvert a PFX file tooa PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="ce42c-143">Další informace najdete v tématu hello [OpenSSL dokumentaci](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="ce42c-143">For more information, see hello [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="ce42c-144">Problémy s připojením</span><span class="sxs-lookup"><span data-stu-id="ce42c-144">Connection issues</span></span>

<span data-ttu-id="ce42c-145">Některé operace může generovat hello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="ce42c-145">Some operations might generate hello following message:</span></span>

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="ce42c-146">Ověřte, že tento hello zadaný koncový bod clusteru je k dispozici a naslouchá.</span><span class="sxs-lookup"><span data-stu-id="ce42c-146">Verify that hello specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="ce42c-147">Také ověřte, že hello, které jsou k dispozici v Service Fabric Explorer uživatelského rozhraní, hostitele a portu.</span><span class="sxs-lookup"><span data-stu-id="ce42c-147">Also, verify that hello Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="ce42c-148">koncový bod hello tooupdate, použijte `sfctl cluster select`.</span><span class="sxs-lookup"><span data-stu-id="ce42c-148">tooupdate hello endpoint, use `sfctl cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="ce42c-149">Podrobné protokoly</span><span class="sxs-lookup"><span data-stu-id="ce42c-149">Detailed logs</span></span>

<span data-ttu-id="ce42c-150">Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému.</span><span class="sxs-lookup"><span data-stu-id="ce42c-150">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="ce42c-151">Je globální konfiguraci `--debug` příznak, který zvyšuje hello podrobností souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="ce42c-151">There is a global `--debug` flag that increases hello verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="ce42c-152">Nápověda k příkazům a jejich syntaxe</span><span class="sxs-lookup"><span data-stu-id="ce42c-152">Command help and syntax</span></span>

<span data-ttu-id="ce42c-153">Pro pomoc s konkrétní příkaz nebo skupinu příkazů, použijte hello `-h` příznak:</span><span class="sxs-lookup"><span data-stu-id="ce42c-153">For help with a specific command or a group of commands, use hello `-h` flag:</span></span>

```azurecli
sfctl application -h
```

<span data-ttu-id="ce42c-154">Další příklad:</span><span class="sxs-lookup"><span data-stu-id="ce42c-154">Another example:</span></span>

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a><span data-ttu-id="ce42c-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ce42c-155">Next steps</span></span>

* [<span data-ttu-id="ce42c-156">Nasazení aplikace s hello příkazového řádku Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ce42c-156">Deploy an application with hello Azure Service Fabric CLI</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="ce42c-157">Začínáme se Service Fabric v Linuxu</span><span class="sxs-lookup"><span data-stu-id="ce42c-157">Get started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
