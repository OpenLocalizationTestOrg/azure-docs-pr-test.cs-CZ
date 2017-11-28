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
# <a name="azure-service-fabric-and-azure-cli-20"></a><span data-ttu-id="6fb05-104">Azure Service Fabric a Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6fb05-104">Azure Service Fabric and Azure CLI 2.0</span></span>

<span data-ttu-id="6fb05-105">Hello nástroj příkazového řádku Azure (Azure CLI) verze 2.0 obsahuje příkazy toohelp spravovat Azure Service Fabric clustery.</span><span class="sxs-lookup"><span data-stu-id="6fb05-105">hello Azure command-line tool (Azure CLI) version 2.0 includes commands toohelp you manage Azure Service Fabric clusters.</span></span> <span data-ttu-id="6fb05-106">Zjistěte, jak tooget pracovat s Azure CLI a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6fb05-106">Learn how tooget started with Azure CLI and Service Fabric.</span></span>

## <a name="install-azure-cli-20"></a><span data-ttu-id="6fb05-107">Instalace Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="6fb05-107">Install Azure CLI 2.0</span></span>

<span data-ttu-id="6fb05-108">Můžete použít příkazy toointeract 2.0 rozhraní příkazového řádku Azure s a správě clusterů Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6fb05-108">You can use Azure CLI 2.0 commands toointeract with and manage Service Fabric clusters.</span></span> <span data-ttu-id="6fb05-109">tooget hello nejnovější verzi rozhraní příkazového řádku Azure, postupujte podle hello [Azure CLI 2.0 standardního procesu instalace](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6fb05-109">tooget hello latest version of Azure CLI, follow hello [Azure CLI 2.0 standard installation process](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="6fb05-110">Další informace najdete v tématu hello [přehled Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6fb05-110">For more information, see hello [Azure CLI 2.0 overview](https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="azure-cli-syntax"></a><span data-ttu-id="6fb05-111">Syntaxe Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6fb05-111">Azure CLI syntax</span></span>

<span data-ttu-id="6fb05-112">V Azure CLI mají všechny příkazy Service Fabric předponu `az sf`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-112">In Azure CLI, all Service Fabric commands are prefixed with `az sf`.</span></span> <span data-ttu-id="6fb05-113">Obecné informace o hello příkazy, které můžete použít, použijte `az sf -h`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-113">For general information about hello commands you can use, use `az sf -h`.</span></span> <span data-ttu-id="6fb05-114">Nápovědu ke konkrétnímu příkazu získáte pomocí příkazu `az sf <command> -h`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-114">For help with a single command, use `az sf <command> -h`.</span></span>

<span data-ttu-id="6fb05-115">Příkazy Service Fabric v Azure CLI používají tento vzorec pojmenování:</span><span class="sxs-lookup"><span data-stu-id="6fb05-115">Service Fabric commands in Azure CLI follow this naming pattern:</span></span>

```azurecli
az sf <object> <action>
```

<span data-ttu-id="6fb05-116">`<object>`hello cíl pro `<action>`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-116">`<object>` is hello target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="6fb05-117">Výběr clusteru</span><span class="sxs-lookup"><span data-stu-id="6fb05-117">Select a cluster</span></span>

<span data-ttu-id="6fb05-118">Před provedením jakékoli operace, je nutné vybrat tooconnect clusteru k.</span><span class="sxs-lookup"><span data-stu-id="6fb05-118">Before you perform any operations, you must select a cluster tooconnect to.</span></span> <span data-ttu-id="6fb05-119">Příklad najdete v tématu hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="6fb05-119">For an example, see hello following code.</span></span> <span data-ttu-id="6fb05-120">Hello kód se připojí tooan zabezpečená clusteru.</span><span class="sxs-lookup"><span data-stu-id="6fb05-120">hello code connects tooan unsecured cluster.</span></span>

> [!WARNING]
> <span data-ttu-id="6fb05-121">Nepoužívejte nezabezpečené clustery Service Fabric v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6fb05-121">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="6fb05-122">koncový bod clusteru Hello musí začínat řetězcem `http` nebo `https`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-122">hello cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="6fb05-123">Musí zahrnovat port hello hello HTTP brány.</span><span class="sxs-lookup"><span data-stu-id="6fb05-123">It must include hello port for hello HTTP gateway.</span></span> <span data-ttu-id="6fb05-124">Hello port a adresa hello jsou stejné jako hello URL Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="6fb05-124">hello port and address are hello same as hello Service Fabric Explorer URL.</span></span>

<span data-ttu-id="6fb05-125">Pro clustery zabezpečené pomocí certifikátu můžete použít buď nešifrované soubory .pem, nebo soubory .crt a .key.</span><span class="sxs-lookup"><span data-stu-id="6fb05-125">For clusters that are secured with a certificate, you can use either unencrypted .pem files, or .crt and .key files.</span></span> <span data-ttu-id="6fb05-126">Například:</span><span class="sxs-lookup"><span data-stu-id="6fb05-126">For example:</span></span>

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="6fb05-127">Další informace najdete v tématu [clusteru zabezpečené Azure Service Fabric připojit tooa](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="6fb05-127">For more information, see [Connect tooa secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6fb05-128">Hello `select` příkaz nebude fungovat u všech požadavků před vrátí.</span><span class="sxs-lookup"><span data-stu-id="6fb05-128">hello `select` command doesn't act on any requests before it returns.</span></span> <span data-ttu-id="6fb05-129">tooverify, že jste určili clusteru správně, použijte příkaz jako `az sf cluster health`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-129">tooverify that you've specified a cluster correctly, use a command like `az sf cluster health`.</span></span> <span data-ttu-id="6fb05-130">Ověřte, že příkaz hello nevrací žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="6fb05-130">Verify that hello command doesn't return any errors.</span></span>

## <a name="basic-operations"></a><span data-ttu-id="6fb05-131">Základní operace</span><span class="sxs-lookup"><span data-stu-id="6fb05-131">Basic operations</span></span>

<span data-ttu-id="6fb05-132">Informace o připojení ke clusteru se uchovávají napříč více relacemi Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="6fb05-132">Cluster connection information persists across multiple Azure CLI sessions.</span></span> <span data-ttu-id="6fb05-133">Po vybrání clusteru Service Fabric, můžete spustit všechny příkazy Service Fabric v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6fb05-133">After you select a Service Fabric cluster, you can run any Service Fabric command on hello cluster.</span></span>

<span data-ttu-id="6fb05-134">Například tooget hello stav clusteru Service Fabric, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6fb05-134">For example, tooget hello Service Fabric cluster health state, use hello following command:</span></span>

```azurecli
az sf cluster health
```

<span data-ttu-id="6fb05-135">příkaz Hello způsobí hello následující výstup (za předpokladu, že výstup JSON je zadaný v konfiguraci rozhraní příkazového řádku Azure hello):</span><span class="sxs-lookup"><span data-stu-id="6fb05-135">hello command results in hello following output (assuming that JSON output is specified in hello Azure CLI configuration):</span></span>

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

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="6fb05-136">Tipy a řešení potíží</span><span class="sxs-lookup"><span data-stu-id="6fb05-136">Tips and troubleshooting</span></span>

<span data-ttu-id="6fb05-137">Můžete se setkat hello následující informace, které jsou užitečné, pokud narazíte na problémy při použití příkazy Service Fabric v rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6fb05-137">You might find hello following information helpful if you run into issues while using Service Fabric commands in Azure CLI.</span></span>

### <a name="convert-a-certificate-from-pfx-toopem-format"></a><span data-ttu-id="6fb05-138">Převést certifikátu z formátu PFX tooPEM</span><span class="sxs-lookup"><span data-stu-id="6fb05-138">Convert a certificate from PFX tooPEM format</span></span>

<span data-ttu-id="6fb05-139">Azure CLI podporuje certifikáty na straně klienta v podobě souborů PEM (s příponou .pem).</span><span class="sxs-lookup"><span data-stu-id="6fb05-139">Azure CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="6fb05-140">Pokud používáte soubory PFX ze systému Windows, je nutné převést formát tooPEM tyto certifikáty.</span><span class="sxs-lookup"><span data-stu-id="6fb05-140">If you use PFX files from Windows, you must convert those certificates tooPEM format.</span></span> <span data-ttu-id="6fb05-141">tooconvert soubor PEM tooa souboru PFX použijte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6fb05-141">tooconvert a PFX file tooa PEM file, use hello following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="6fb05-142">Další informace najdete v tématu hello [OpenSSL dokumentaci](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="6fb05-142">For more information, see hello [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="6fb05-143">Problémy s připojením</span><span class="sxs-lookup"><span data-stu-id="6fb05-143">Connection issues</span></span>

<span data-ttu-id="6fb05-144">Některé operace může generovat hello následující zprávou:</span><span class="sxs-lookup"><span data-stu-id="6fb05-144">Some operations might generate hello following message:</span></span>

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="6fb05-145">Ověřte, že tento hello zadaný koncový bod clusteru je k dispozici a naslouchá.</span><span class="sxs-lookup"><span data-stu-id="6fb05-145">Verify that hello specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="6fb05-146">Také ověřte, že hello, které jsou k dispozici v Service Fabric Explorer uživatelského rozhraní, hostitele a portu.</span><span class="sxs-lookup"><span data-stu-id="6fb05-146">Also, verify that hello Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="6fb05-147">koncový bod hello tooupdate, použijte `az sf cluster select`.</span><span class="sxs-lookup"><span data-stu-id="6fb05-147">tooupdate hello endpoint, use `az sf cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="6fb05-148">Podrobné protokoly</span><span class="sxs-lookup"><span data-stu-id="6fb05-148">Detailed logs</span></span>

<span data-ttu-id="6fb05-149">Podrobné protokoly jsou často užitečné při ladění nebo hlášení problému.</span><span class="sxs-lookup"><span data-stu-id="6fb05-149">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="6fb05-150">Rozhraní příkazového řádku Azure poskytuje globální konfiguraci `--debug` příznak, který zvyšuje hello podrobností souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="6fb05-150">Azure CLI offers a global `--debug` flag that increases hello verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="6fb05-151">Nápověda k příkazům a jejich syntaxe</span><span class="sxs-lookup"><span data-stu-id="6fb05-151">Command help and syntax</span></span>

<span data-ttu-id="6fb05-152">Postupujte podle příkazy Service Fabric hello stejné konvence jako rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6fb05-152">Service Fabric commands follow hello same conventions as Azure CLI.</span></span> <span data-ttu-id="6fb05-153">Pro pomoc s konkrétní příkaz nebo skupinu příkazů, použijte hello `-h` příznak:</span><span class="sxs-lookup"><span data-stu-id="6fb05-153">For help with a specific command or a group of commands, use hello `-h` flag:</span></span>

```azurecli
az sf application -h
```

<span data-ttu-id="6fb05-154">Tady je další příklad:</span><span class="sxs-lookup"><span data-stu-id="6fb05-154">Here's another example:</span></span>

```azurecli
az sf application create -h
```
