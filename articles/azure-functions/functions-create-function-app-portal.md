---
title: "aaaCreate funkce aplikace z hello portálu Azure | Microsoft Docs"
description: "Vytvořte novou aplikaci funkce ve službě Azure App Service z portálu hello."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: c531fc71c798edf22e25a5f4b79c15413809dc86
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-app-from-hello-azure-portal"></a><span data-ttu-id="76ece-103">Vytvoření funkce aplikace hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="76ece-103">Create a function app from hello Azure portal</span></span>

<span data-ttu-id="76ece-104">Aplikace Azure funkce používá infrastrukturu Azure App Service hello.</span><span class="sxs-lookup"><span data-stu-id="76ece-104">Azure Function Apps uses hello Azure App Service infrastructure.</span></span> <span data-ttu-id="76ece-105">Toto téma ukazuje, jak toocreate funkce aplikace v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="76ece-105">This topic shows you how toocreate a function app in hello Azure portal.</span></span> <span data-ttu-id="76ece-106">Funkce aplikace je hello kontejner, který je hostitelem hello provádění jednotlivých funkcí.</span><span class="sxs-lookup"><span data-stu-id="76ece-106">A function app is hello container that hosts hello execution of individual functions.</span></span> <span data-ttu-id="76ece-107">Když vytvoříte aplikaci funkce v hello hostování plán služby App Service, funkce aplikace můžete používat všechny funkce hello služby App Service.</span><span class="sxs-lookup"><span data-stu-id="76ece-107">When you create a function app in hello App Service hosting plan, your function app can use all hello features of App Service.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="76ece-108">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="76ece-108">Create a function app</span></span>

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="76ece-109">Když vytvoříte aplikaci function app, zadejte platný **název aplikace**, který může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="76ece-109">When you create a function app, supply a valid **App name**, which can contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="76ece-110">Podtržítko (**_**) není povolené znak.</span><span class="sxs-lookup"><span data-stu-id="76ece-110">Underscore (**_**) is not an allowed character.</span></span>

<span data-ttu-id="76ece-111">Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="76ece-111">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="76ece-112">Váš název účtu úložiště musí být jedinečný v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="76ece-112">Your storage account name must be unique within Azure.</span></span> 

<span data-ttu-id="76ece-113">Po vytvoření aplikace hello funkce vytvořením jednotlivých funkcí na jeden nebo více různých jazyků.</span><span class="sxs-lookup"><span data-stu-id="76ece-113">After hello function app is created, you can create individual functions in one or more different languages.</span></span> <span data-ttu-id="76ece-114">Vytvoření funkce [pomocí portálu hello](functions-create-first-azure-function.md#create-function), [průběžné nasazování](functions-continuous-deployment.md), nebo pomocí [odesílání spolu s FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span><span class="sxs-lookup"><span data-stu-id="76ece-114">Create functions [by using hello portal](functions-create-first-azure-function.md#create-function), [continuous deployment](functions-continuous-deployment.md), or by [uploading with FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span></span>

## <a name="service-plans"></a><span data-ttu-id="76ece-115">Plány služeb</span><span class="sxs-lookup"><span data-stu-id="76ece-115">Service plans</span></span>

<span data-ttu-id="76ece-116">Azure Functions nabízí dva druhy různých služeb: plánování využívání a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="76ece-116">Azure Functions has two different service plans: Consumption plan and App Service plan.</span></span> <span data-ttu-id="76ece-117">plánu spotřeby Hello automaticky přiděluje výpočetní výkon, pokud kód je spuštěný, měřítka na více systémů jako nezbytné toohandle zatížení a potom měřítka – v kódu není.</span><span class="sxs-lookup"><span data-stu-id="76ece-117">hello Consumption plan automatically allocates compute power when your code is running, scales-out as necessary toohandle load, and then scales-in when code is not running.</span></span> <span data-ttu-id="76ece-118">Hello plán služby App Service poskytuje funkce aplikace přístup tooall hello zařízení služby App Service.</span><span class="sxs-lookup"><span data-stu-id="76ece-118">hello App Service plan gives your function app access tooall hello facilities of App Service.</span></span> <span data-ttu-id="76ece-119">Když je vytvořena funkce aplikace a nelze ji změnit, musíte zvolit plán služby.</span><span class="sxs-lookup"><span data-stu-id="76ece-119">You must choose your service plan when your function app is created, and it cannot currently be changed.</span></span> <span data-ttu-id="76ece-120">Další informace najdete v tématu [zvolte Azure Functions hostování plán](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="76ece-120">For more information, see [Choose an Azure Functions hosting plan](functions-scale.md).</span></span>

<span data-ttu-id="76ece-121">Pokud plánujete toorun funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán s menším počtem jader.</span><span class="sxs-lookup"><span data-stu-id="76ece-121">If you are planning toorun JavaScript functions on an App Service plan, you should choose a plan with fewer cores.</span></span> <span data-ttu-id="76ece-122">Další informace najdete v tématu hello [referenční dokumentace technologie JavaScript pro funkce](functions-reference-node.md#choose-single-core-app-service-plans).</span><span class="sxs-lookup"><span data-stu-id="76ece-122">For more information, see hello [JavaScript reference for Functions](functions-reference-node.md#choose-single-core-app-service-plans).</span></span>

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a><span data-ttu-id="76ece-123">Požadavky na účet úložiště</span><span class="sxs-lookup"><span data-stu-id="76ece-123">Storage account requirements</span></span>

<span data-ttu-id="76ece-124">Když vytváříte aplikaci funkce ve službě App Service, musíte vytvořit nebo připojit tooa pro obecné účely Azure účet úložiště, který podporuje úložiště Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="76ece-124">When creating a function app in App Service, you must create or link tooa general-purpose Azure Storage account that supports Blob, Queue, and Table storage.</span></span> <span data-ttu-id="76ece-125">Funkce interně používá úložiště pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce.</span><span class="sxs-lookup"><span data-stu-id="76ece-125">Internally, Functions uses Storage for operations such as managing triggers and logging function executions.</span></span> <span data-ttu-id="76ece-126">Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště Azure Premium Storage a účty úložiště pro obecné účely replikaci ZRS.</span><span class="sxs-lookup"><span data-stu-id="76ece-126">Some storage accounts do not support queues and tables, such as blob-only storage accounts, Azure Premium Storage, and general-purpose storage accounts with ZRS replication.</span></span> <span data-ttu-id="76ece-127">Tyto účty jsou filtrovány mimo v okně účtu úložiště hello při vytváření aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="76ece-127">These accounts are filtered out of from hello Storage Account blade when creating a function app.</span></span>

>[!NOTE]
><span data-ttu-id="76ece-128">Při použití hello spotřeba hostingový plán, funkce kód a vazba konfiguračních souborů jsou uložené v Azure File storage v účtu úložiště hlavní hello.</span><span class="sxs-lookup"><span data-stu-id="76ece-128">When using hello Consumption hosting plan, your function code and binding configuration files are stored in Azure File storage in hello main storage account.</span></span> <span data-ttu-id="76ece-129">Pokud odstraníte účet úložiště hlavní hello, tento obsah se odstraní a nelze jej obnovit.</span><span class="sxs-lookup"><span data-stu-id="76ece-129">When you delete hello main storage account, this content is deleted and cannot be recovered.</span></span>

<span data-ttu-id="76ece-130">toolearn Další informace o typy účtů úložiště, najdete v části [Představení služby úložiště Azure hello](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span><span class="sxs-lookup"><span data-stu-id="76ece-130">toolearn more about storage account types, see [Introducing hello Azure Storage Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="76ece-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="76ece-131">Next steps</span></span>

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



