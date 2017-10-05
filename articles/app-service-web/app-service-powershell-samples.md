---
title: "Ukázky Azure PowerShell - služby App Service | Microsoft Docs"
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
ms.openlocfilehash: 3254fdd57cfcd170f22374c1e3b058e6081d8e8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples"></a><span data-ttu-id="02291-103">Ukázky Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="02291-103">Azure PowerShell Samples</span></span>

<span data-ttu-id="02291-104">Následující tabulka obsahuje odkazy na bash skripty, které jsou vytvořené pomocí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="02291-104">The following table includes links to bash scripts built using the Azure PowerShell.</span></span>

| | |
|-|-|
|<span data-ttu-id="02291-105">**Vytvoření aplikace**</span><span class="sxs-lookup"><span data-stu-id="02291-105">**Create app**</span></span>||
| [<span data-ttu-id="02291-106">Vytvoření webové aplikace s nasazením z Githubu</span><span class="sxs-lookup"><span data-stu-id="02291-106">Create a web app with deployment from GitHub</span></span>](./scripts/app-service-powershell-deploy-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="02291-107">Vytvoří Azure webové aplikace, která vrátí kód z Githubu.</span><span class="sxs-lookup"><span data-stu-id="02291-107">Creates an Azure web app which pulls code from GitHub.</span></span> |
| [<span data-ttu-id="02291-108">Vytvoření webové aplikace s průběžným nasazováním z Githubu</span><span class="sxs-lookup"><span data-stu-id="02291-108">Create a web app with continuous deployment from GitHub</span></span>](./scripts/app-service-powershell-continuous-deployment-github.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="02291-109">Vytvoří Azure webové aplikace, které průběžně nasadí kód z Githubu.</span><span class="sxs-lookup"><span data-stu-id="02291-109">Creates an Azure web app which continuously deploys code from GitHub.</span></span> |
| [<span data-ttu-id="02291-110">Vytvoření webové aplikace a nasazení kódu do serveru FTP</span><span class="sxs-lookup"><span data-stu-id="02291-110">Create a web app and deploy code with FTP</span></span>](./scripts/app-service-powershell-deploy-ftp.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="02291-111">Vytvoří službě Azure web app a nahrávání souborů z místního adresáře pomocí protokolu FTP.</span><span class="sxs-lookup"><span data-stu-id="02291-111">Creates an Azure web app and upload files from a local directory using FTP.</span></span> |
| [<span data-ttu-id="02291-112">Vytvoření webové aplikace a nasazení kódu z místního úložiště Git</span><span class="sxs-lookup"><span data-stu-id="02291-112">Create a web app and deploy code from a local Git repository</span></span>](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="02291-113">Vytvoří webové aplikace Azure a nakonfiguruje nabízené kód z místního úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="02291-113">Creates an Azure web app and configures code push from a local Git repository.</span></span> |
| [<span data-ttu-id="02291-114">Vytvoření webové aplikace a nasazení kódu do přípravného prostředí</span><span class="sxs-lookup"><span data-stu-id="02291-114">Create a web app and deploy code to a staging environment</span></span>](./scripts/app-service-powershell-deploy-staging-environment.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="02291-115">Vytvoří se nasazovací slot pro přípravu změny kódu webové aplikace Azure.</span><span class="sxs-lookup"><span data-stu-id="02291-115">Creates an Azure web app with a deployment slot for staging code changes.</span></span> |
|<span data-ttu-id="02291-116">**Konfigurace aplikace**</span><span class="sxs-lookup"><span data-stu-id="02291-116">**Configure app**</span></span>||
| [<span data-ttu-id="02291-117">Mapování vlastní domény na webovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="02291-117">Map a custom domain to a web app</span></span>](./scripts/app-service-powershell-configure-custom-domain.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="02291-118">Vytvoří webové aplikace Azure a mapuje vlastního názvu domény do ní.</span><span class="sxs-lookup"><span data-stu-id="02291-118">Creates an Azure web app and maps a custom domain name to it.</span></span> |
| [<span data-ttu-id="02291-119">Vlastní certifikát SSL vazbu do webové aplikace</span><span class="sxs-lookup"><span data-stu-id="02291-119">Bind a custom SSL certificate to a web app</span></span>](./scripts/app-service-powershell-configure-ssl-certificate.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="02291-120">Vytvoří webové aplikace Azure a vytvoří vazbu certifikátu SSL vlastního názvu domény.</span><span class="sxs-lookup"><span data-stu-id="02291-120">Creates an Azure web app and binds the SSL certificate of a custom domain name to it.</span></span> |
|<span data-ttu-id="02291-121">**Škálování aplikace**</span><span class="sxs-lookup"><span data-stu-id="02291-121">**Scale app**</span></span>||
| [<span data-ttu-id="02291-122">Ruční škálování webové aplikace</span><span class="sxs-lookup"><span data-stu-id="02291-122">Scale a web app manually</span></span>](./scripts/app-service-powershell-scale-manual.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="02291-123">Vytvoří webové aplikace Azure a škáluje napříč 2 instance.</span><span class="sxs-lookup"><span data-stu-id="02291-123">Creates an Azure web app and scales it across 2 instances.</span></span> |
| [<span data-ttu-id="02291-124">Škálování webové aplikace po celém světě s využitím architektury s vysokou dostupností</span><span class="sxs-lookup"><span data-stu-id="02291-124">Scale a web app worldwide with a high-availability architecture</span></span>](./scripts/app-service-powershell-scale-high-availability.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="02291-125">Vytvoří dvě službě Azure web apps ve dvou různých zeměpisných oblastech a jsou pak dostupné přes jeden koncový bod pomocí Azure Traffic Manager.</span><span class="sxs-lookup"><span data-stu-id="02291-125">Creates two Azure web apps in two different geographical regions and makes them available through a single endpoint using Azure Traffic Manager.</span></span> |
|<span data-ttu-id="02291-126">**Připojení aplikace k prostředkům**</span><span class="sxs-lookup"><span data-stu-id="02291-126">**Connect app to resources**</span></span>||
| [<span data-ttu-id="02291-127">Webovou aplikaci připojit k databázi SQL</span><span class="sxs-lookup"><span data-stu-id="02291-127">Connect a web app to a SQL Database</span></span>](./scripts/app-service-powershell-connect-to-sql.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="02291-128">Vytvoří webové aplikace Azure a SQL database a potom přidá připojovací řetězec databáze nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="02291-128">Creates an Azure web app and a SQL database, then adds the database connection string to the app settings.</span></span> |
| [<span data-ttu-id="02291-129">Připojení webové aplikace k účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="02291-129">Connect a web app to a storage account</span></span>](./scripts/app-service-powershell-connect-to-storage.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="02291-130">Vytvoří webové aplikace Azure a účet úložiště a potom přidá připojovacího řetězce úložiště nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="02291-130">Creates an Azure web app and a storage account, then adds the storage connection string to the app settings.</span></span> |
|<span data-ttu-id="02291-131">**Monitorování aplikace**</span><span class="sxs-lookup"><span data-stu-id="02291-131">**Monitor app**</span></span>||
| [<span data-ttu-id="02291-132">Monitorování webové aplikace pomocí protokolů webového serveru</span><span class="sxs-lookup"><span data-stu-id="02291-132">Monitor a web app with web server logs</span></span>](./scripts/app-service-powershell-monitor.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="02291-133">Vytvoří webové aplikace Azure, povolí protokolování pro něj a stáhne protokoly do místního počítače.</span><span class="sxs-lookup"><span data-stu-id="02291-133">Creates an Azure web app, enables logging for it, and downloads the logs to your local machine.</span></span> |
| | |
