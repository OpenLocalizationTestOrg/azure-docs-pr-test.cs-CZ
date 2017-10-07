---
title: "Funkce aaaAzure prostředí cloudu (Preview) | Microsoft Docs"
description: "Přehled funkcí Azure cloudové prostředí"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 65482ca6caeac01dda18a6b12eabe943e3d68a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="features-and-tools-for-azure-cloud-shell"></a><span data-ttu-id="867ae-103">Funkce a nástroje pro prostředí cloudu Azure</span><span class="sxs-lookup"><span data-stu-id="867ae-103">Features and Tools for Azure Cloud Shell</span></span>
<span data-ttu-id="867ae-104">Prostředí Azure Cloud je toomanage prostředí prostředí založené na prohlížeči a vyvíjet prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="867ae-104">Azure Cloud Shell is a browser-based shell experience toomanage and develop Azure resources.</span></span>

<span data-ttu-id="867ae-105">Cloudové prostředí nabízí přístupných na prohlížeč, předem nakonfigurované prostředí prostředí pro správu prostředků Azure bez režie hello instalaci, správu verzí, a musí jí spravovat počítač s sami.</span><span class="sxs-lookup"><span data-stu-id="867ae-105">Cloud Shell offers a browser-accessible, pre-configured shell experience for managing Azure resources without hello overhead of installing, versioning, and maintaining a machine yourself.</span></span>

<span data-ttu-id="867ae-106">Prostředí cloudu se zřizuje počítače na základě požadavků a proto nebude stav počítače zachová napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="867ae-106">Cloud Shell provisions machines on a per-request basis and as a result machine state will not persist across sessions.</span></span> <span data-ttu-id="867ae-107">Vzhledem k tomu, že pro interaktivní relace je integrované cloudové prostředí, prostředí shell automaticky ukončit po 20 minutách nečinnosti prostředí.</span><span class="sxs-lookup"><span data-stu-id="867ae-107">Since Cloud Shell is built for interactive sessions, shells automatically terminate after 20 minutes of shell inactivity.</span></span>

## <a name="bash-in-cloud-shell"></a><span data-ttu-id="867ae-108">Bash v prostředí cloudu</span><span class="sxs-lookup"><span data-stu-id="867ae-108">Bash in Cloud Shell</span></span>
### <a name="tools"></a><span data-ttu-id="867ae-109">Nástroje</span><span class="sxs-lookup"><span data-stu-id="867ae-109">Tools</span></span>
|<span data-ttu-id="867ae-110">Kategorie</span><span class="sxs-lookup"><span data-stu-id="867ae-110">Category</span></span>   |<span data-ttu-id="867ae-111">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="867ae-111">Name</span></span>   |
|---|---|
|<span data-ttu-id="867ae-112">Překladač prostředí Linux</span><span class="sxs-lookup"><span data-stu-id="867ae-112">Linux shell interpreter</span></span>|<span data-ttu-id="867ae-113">Bash</span><span class="sxs-lookup"><span data-stu-id="867ae-113">Bash</span></span><br> <span data-ttu-id="867ae-114">Zo</span><span class="sxs-lookup"><span data-stu-id="867ae-114">sh</span></span>               |
|<span data-ttu-id="867ae-115">Nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="867ae-115">Azure tools</span></span>            |<span data-ttu-id="867ae-116">[Rozhraní příkazového řádku Azure 2.0](https://github.com/Azure/azure-cli) a [1.0](https://github.com/Azure/azure-xplat-cli)</span><span class="sxs-lookup"><span data-stu-id="867ae-116">[Azure CLI 2.0](https://github.com/Azure/azure-cli) and [1.0](https://github.com/Azure/azure-xplat-cli)</span></span><br> [<span data-ttu-id="867ae-117">AzCopy</span><span class="sxs-lookup"><span data-stu-id="867ae-117">AzCopy</span></span>](https://docs.microsoft.com/azure/storage/storage-use-azcopy)<br> [<span data-ttu-id="867ae-118">Batch Shipyard</span><span class="sxs-lookup"><span data-stu-id="867ae-118">Batch Shipyard</span></span>](https://github.com/Azure/batch-shipyard)     |
|<span data-ttu-id="867ae-119">Textové editory</span><span class="sxs-lookup"><span data-stu-id="867ae-119">Text editors</span></span>           |<span data-ttu-id="867ae-120">VIM</span><span class="sxs-lookup"><span data-stu-id="867ae-120">vim</span></span><br> <span data-ttu-id="867ae-121">nano</span><span class="sxs-lookup"><span data-stu-id="867ae-121">nano</span></span><br> <span data-ttu-id="867ae-122">EMACS</span><span class="sxs-lookup"><span data-stu-id="867ae-122">emacs</span></span>       |
|<span data-ttu-id="867ae-123">Správa zdrojového kódu</span><span class="sxs-lookup"><span data-stu-id="867ae-123">Source control</span></span>         |<span data-ttu-id="867ae-124">Git</span><span class="sxs-lookup"><span data-stu-id="867ae-124">git</span></span>                    |
|<span data-ttu-id="867ae-125">Nástroje sestavení</span><span class="sxs-lookup"><span data-stu-id="867ae-125">Build tools</span></span>            |<span data-ttu-id="867ae-126">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="867ae-126">make</span></span><br> <span data-ttu-id="867ae-127">maven</span><span class="sxs-lookup"><span data-stu-id="867ae-127">maven</span></span><br> <span data-ttu-id="867ae-128">npm</span><span class="sxs-lookup"><span data-stu-id="867ae-128">npm</span></span><br> <span data-ttu-id="867ae-129">PIP</span><span class="sxs-lookup"><span data-stu-id="867ae-129">pip</span></span>         |
|<span data-ttu-id="867ae-130">Kontejnery</span><span class="sxs-lookup"><span data-stu-id="867ae-130">Containers</span></span>             |<span data-ttu-id="867ae-131">[Rozhraní příkazového řádku dockeru](https://github.com/docker/cli)/[počítač Docker](https://github.com/docker/machine)</span><span class="sxs-lookup"><span data-stu-id="867ae-131">[Docker CLI](https://github.com/docker/cli)/[Docker Machine](https://github.com/docker/machine)</span></span><br> [<span data-ttu-id="867ae-132">Kubectl</span><span class="sxs-lookup"><span data-stu-id="867ae-132">Kubectl</span></span>](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [<span data-ttu-id="867ae-133">Koncept</span><span class="sxs-lookup"><span data-stu-id="867ae-133">Draft</span></span>](https://github.com/Azure/draft)<br> [<span data-ttu-id="867ae-134">ROZHRANÍ PŘÍKAZOVÉHO ŘÁDKU DC/OS</span><span class="sxs-lookup"><span data-stu-id="867ae-134">DC/OS CLI</span></span>](https://github.com/dcos/dcos-cli)         |
|<span data-ttu-id="867ae-135">Databáze</span><span class="sxs-lookup"><span data-stu-id="867ae-135">Databases</span></span>              |<span data-ttu-id="867ae-136">MySQL klienta</span><span class="sxs-lookup"><span data-stu-id="867ae-136">MySQL client</span></span><br> <span data-ttu-id="867ae-137">PostgreSql klienta</span><span class="sxs-lookup"><span data-stu-id="867ae-137">PostgreSql client</span></span><br> [<span data-ttu-id="867ae-138">Nástroj SQLCMD</span><span class="sxs-lookup"><span data-stu-id="867ae-138">sqlcmd Utility</span></span>](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [<span data-ttu-id="867ae-139">Tvůrce MSSQL skriptů</span><span class="sxs-lookup"><span data-stu-id="867ae-139">mssql-scripter</span></span>](https://github.com/Microsoft/sql-xplat-cli) |
|<span data-ttu-id="867ae-140">Ostatní</span><span class="sxs-lookup"><span data-stu-id="867ae-140">Other</span></span>                  |<span data-ttu-id="867ae-141">iPython klienta</span><span class="sxs-lookup"><span data-stu-id="867ae-141">iPython Client</span></span><br> [<span data-ttu-id="867ae-142">Cloud Foundry rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="867ae-142">Cloud Foundry CLI</span></span>](https://github.com/cloudfoundry/cli)<br> |

### <a name="language-support"></a><span data-ttu-id="867ae-143">Podpora jazyků</span><span class="sxs-lookup"><span data-stu-id="867ae-143">Language support</span></span>
|<span data-ttu-id="867ae-144">Jazyk</span><span class="sxs-lookup"><span data-stu-id="867ae-144">Language</span></span>   |<span data-ttu-id="867ae-145">Verze</span><span class="sxs-lookup"><span data-stu-id="867ae-145">Version</span></span>   |
|---|---|
|<span data-ttu-id="867ae-146">.NET</span><span class="sxs-lookup"><span data-stu-id="867ae-146">.NET</span></span>       |<span data-ttu-id="867ae-147">1.01</span><span class="sxs-lookup"><span data-stu-id="867ae-147">1.01</span></span>       |
|<span data-ttu-id="867ae-148">Přejít</span><span class="sxs-lookup"><span data-stu-id="867ae-148">Go</span></span>         |<span data-ttu-id="867ae-149">1.7</span><span class="sxs-lookup"><span data-stu-id="867ae-149">1.7</span></span>        |
|<span data-ttu-id="867ae-150">Java</span><span class="sxs-lookup"><span data-stu-id="867ae-150">Java</span></span>       |<span data-ttu-id="867ae-151">1.8</span><span class="sxs-lookup"><span data-stu-id="867ae-151">1.8</span></span>        |
|<span data-ttu-id="867ae-152">Node.js</span><span class="sxs-lookup"><span data-stu-id="867ae-152">Node.js</span></span>    |<span data-ttu-id="867ae-153">6.9.4</span><span class="sxs-lookup"><span data-stu-id="867ae-153">6.9.4</span></span>      |
|<span data-ttu-id="867ae-154">Python</span><span class="sxs-lookup"><span data-stu-id="867ae-154">Python</span></span>     |<span data-ttu-id="867ae-155">2.7 a 3.5 (výchozí)</span><span class="sxs-lookup"><span data-stu-id="867ae-155">2.7 and 3.5 (default)</span></span>|

## <a name="secure-automatic-authentication"></a><span data-ttu-id="867ae-156">Zabezpečení automatické ověřování</span><span class="sxs-lookup"><span data-stu-id="867ae-156">Secure automatic authentication</span></span>
<span data-ttu-id="867ae-157">Cloudové prostředí bezpečně a automaticky ověřuje přístup k účtu pro hello 2.0 rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="867ae-157">Cloud Shell securely and automatically authenticates account access for hello Azure CLI 2.0.</span></span>

## <a name="azure-files-persistence"></a><span data-ttu-id="867ae-158">Azure trvalost soubory</span><span class="sxs-lookup"><span data-stu-id="867ae-158">Azure Files persistence</span></span>
<span data-ttu-id="867ae-159">Vzhledem k tomu, že cloudové prostředí je přidělena na základě požadavků pomocí dočasného počítače, nejsou soubory mimo vašemu $Home a počítač stavu trvalé napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="867ae-159">Since Cloud Shell is allocated on a per-request basis using a temporary machine, files outside of your $Home and machine state are not persisted across sessions.</span></span>
<span data-ttu-id="867ae-160">soubory toopersist napříč relacemi, cloudové prostředí nevystavíte slabé stránky zabezpečení prostřednictvím připojení Azure file sdílíte při prvním spuštění.</span><span class="sxs-lookup"><span data-stu-id="867ae-160">toopersist files across sessions, Cloud Shell walks you through attaching an Azure file share on first launch.</span></span>
<span data-ttu-id="867ae-161">Po dokončení cloudové prostředí automaticky připojí úložiště pro všechny budoucí relace.</span><span class="sxs-lookup"><span data-stu-id="867ae-161">Once completed Cloud Shell will automatically attach your storage for all future sessions.</span></span>

[<span data-ttu-id="867ae-162">Další informace o připojení tooCloud sdílené složky Azure file prostředí.</span><span class="sxs-lookup"><span data-stu-id="867ae-162">Learn more about attaching Azure file shares tooCloud Shell.</span></span>](persisting-shell-storage.md)

## <a name="next-steps"></a><span data-ttu-id="867ae-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="867ae-163">Next steps</span></span>
[<span data-ttu-id="867ae-164">Rychlý start cloudové prostředí</span><span class="sxs-lookup"><span data-stu-id="867ae-164">Cloud Shell Quickstart</span></span>](quickstart.md) <br><span data-ttu-id="867ae-165">
[Další informace o rozhraní příkazového řádku Azure 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="867ae-165">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br>