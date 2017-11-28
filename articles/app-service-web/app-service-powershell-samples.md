---
title: "aaaAzure ukázky PowerShell - App Service | Microsoft Docs"
description: "Ukázky Azure PowerShell - služby App Service"
services: app-service
documentationcenter: app-service
author: syntaxc4
manager: erikre
editor: ggailey777
tags: azure-service-management
ms.assetid: b48d1137-8c04-46e0-b430-101e07d7e470
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: app-service
ms.date: 03/08/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: b7b4a030364f797195522c56fbae5b7f530d4d1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples"></a><span data-ttu-id="ebbb9-103">Ukázky Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ebbb9-103">Azure PowerShell Samples</span></span>

<span data-ttu-id="ebbb9-104">Hello následující tabulka obsahuje odkazy toobash skripty vyvíjené hello prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-104">hello following table includes links toobash scripts built using hello Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="ebbb9-105">**Vytvoření aplikace**</span><span class="sxs-lookup"><span data-stu-id="ebbb9-105">**Create app**</span></span>||
| [<span data-ttu-id="ebbb9-106">Vytvoření webové aplikace s nasazením z Githubu</span><span class="sxs-lookup"><span data-stu-id="ebbb9-106">Create a web app with deployment from GitHub</span></span>](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ebbb9-107">Vytvoří Azure webové aplikace, která vrátí kód z Githubu.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-107">Creates an Azure web app which pulls code from GitHub.</span></span> |
| [<span data-ttu-id="ebbb9-108">Vytvoření webové aplikace s průběžným nasazováním z Githubu</span><span class="sxs-lookup"><span data-stu-id="ebbb9-108">Create a web app with continuous deployment from GitHub</span></span>](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ebbb9-109">Vytvoří Azure webové aplikace, které průběžně nasadí kód z Githubu.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-109">Creates an Azure web app which continuously deploys code from GitHub.</span></span> |
| [<span data-ttu-id="ebbb9-110">Vytvoření webové aplikace a nasazení kódu do serveru FTP</span><span class="sxs-lookup"><span data-stu-id="ebbb9-110">Create a web app and deploy code with FTP</span></span>](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ebbb9-111">Vytvoří službě Azure web app a nahrávání souborů z místního adresáře pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-111">Creates an Azure web app and upload files from a local directory using FTP.</span></span> |
| [<span data-ttu-id="ebbb9-112">Vytvoření webové aplikace a nasazení kódu z místního úložiště Git</span><span class="sxs-lookup"><span data-stu-id="ebbb9-112">Create a web app and deploy code from a local Git repository</span></span>](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ebbb9-113">Vytvoří webové aplikace Azure a nakonfiguruje nabízené kód z místního úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-113">Creates an Azure web app and configures code push from a local Git repository.</span></span> |
| [<span data-ttu-id="ebbb9-114">Vytvoření webové aplikace a nasazení kódu tooa pracovní prostředí</span><span class="sxs-lookup"><span data-stu-id="ebbb9-114">Create a web app and deploy code tooa staging environment</span></span>](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ebbb9-115">Vytvoří se nasazovací slot pro přípravu změny kódu webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-115">Creates an Azure web app with a deployment slot for staging code changes.</span></span> |
|<span data-ttu-id="ebbb9-116">**Konfigurace aplikace**</span><span class="sxs-lookup"><span data-stu-id="ebbb9-116">**Configure app**</span></span>||
| [<span data-ttu-id="ebbb9-117">Mapa vlastní domény tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ebbb9-117">Map a custom domain tooa web app</span></span>](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ebbb9-118">Vytvoří webové aplikace Azure a mapuje tooit název vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-118">Creates an Azure web app and maps a custom domain name tooit.</span></span> |
| [<span data-ttu-id="ebbb9-119">Vytvoření vazby vlastní tooa webové aplikace certifikát SSL</span><span class="sxs-lookup"><span data-stu-id="ebbb9-119">Bind a custom SSL certificate tooa web app</span></span>](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ebbb9-120">Vytvoří webové aplikace Azure a vytvoří vazbu certifikátu SSL hello tooit název vlastní doménu.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-120">Creates an Azure web app and binds hello SSL certificate of a custom domain name tooit.</span></span> |
|<span data-ttu-id="ebbb9-121">**Škálování aplikace**</span><span class="sxs-lookup"><span data-stu-id="ebbb9-121">**Scale app**</span></span>||
| [<span data-ttu-id="ebbb9-122">Ruční škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ebbb9-122">Scale a web app manually</span></span>](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ebbb9-123">Vytvoří webové aplikace Azure a škáluje napříč 2 instance.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-123">Creates an Azure web app and scales it across 2 instances.</span></span> |
| [<span data-ttu-id="ebbb9-124">Škálování webové aplikace po celém světě s využitím architektury s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="ebbb9-124">Scale a web app worldwide with a high-availability architecture</span></span>](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ebbb9-125">Vytvoří dvě službě Azure web apps ve dvou různých zeměpisných oblastech a jsou pak dostupné přes jeden koncový bod pomocí Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-125">Creates two Azure web apps in two different geographical regions and makes them available through a single endpoint using Azure Traffic Manager.</span></span> |
|<span data-ttu-id="ebbb9-126">**Připojení aplikace tooresources**</span><span class="sxs-lookup"><span data-stu-id="ebbb9-126">**Connect app tooresources**</span></span>||
| [<span data-ttu-id="ebbb9-127">Připojení webové aplikaci tooa databáze SQL</span><span class="sxs-lookup"><span data-stu-id="ebbb9-127">Connect a web app tooa SQL Database</span></span>](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ebbb9-128">Vytvoří webové aplikace Azure a SQL database a potom přidá hello databáze připojovací řetězec toohello nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-128">Creates an Azure web app and a SQL database, then adds hello database connection string toohello app settings.</span></span> |
| [<span data-ttu-id="ebbb9-129">Připojení účtu úložiště tooa webové aplikace</span><span class="sxs-lookup"><span data-stu-id="ebbb9-129">Connect a web app tooa storage account</span></span>](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="ebbb9-130">Vytvoří webové aplikace Azure a účet úložiště a potom přidá hello úložiště připojovací řetězec toohello nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-130">Creates an Azure web app and a storage account, then adds hello storage connection string toohello app settings.</span></span> |
|<span data-ttu-id="ebbb9-131">**Monitorování aplikace**</span><span class="sxs-lookup"><span data-stu-id="ebbb9-131">**Monitor app**</span></span>||
| [<span data-ttu-id="ebbb9-132">Monitorování webové aplikace pomocí protokolů webového serveru</span><span class="sxs-lookup"><span data-stu-id="ebbb9-132">Monitor a web app with web server logs</span></span>](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="ebbb9-133">Vytvoří webové aplikace Azure, povolí protokolování pro něj a stáhne hello protokoly tooyour místního počítače.</span><span class="sxs-lookup"><span data-stu-id="ebbb9-133">Creates an Azure web app, enables logging for it, and downloads hello logs tooyour local machine.</span></span> |
| | |
