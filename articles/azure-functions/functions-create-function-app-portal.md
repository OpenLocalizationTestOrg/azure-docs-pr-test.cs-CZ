---
title: "Vytvoření aplikace pro funkce na portálu Azure | Microsoft Docs"
description: "Vytvořte novou aplikaci funkce ve službě Azure App Service z portálu."
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
ms.openlocfilehash: 85a88c537415cd6f2b6bc005cc18e3baaa29e9a4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-app-from-the-azure-portal"></a><span data-ttu-id="1253f-103">Vytvoření funkce aplikace z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1253f-103">Create a function app from the Azure portal</span></span>

<span data-ttu-id="1253f-104">Aplikace Azure funkce používá infrastrukturu Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1253f-104">Azure Function Apps uses the Azure App Service infrastructure.</span></span> <span data-ttu-id="1253f-105">Toto téma ukazuje, jak vytvořit aplikaci function app na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1253f-105">This topic shows you how to create a function app in the Azure portal.</span></span> <span data-ttu-id="1253f-106">Funkce aplikace je kontejner, který je hostitelem provádění jednotlivých funkcí.</span><span class="sxs-lookup"><span data-stu-id="1253f-106">A function app is the container that hosts the execution of individual functions.</span></span> <span data-ttu-id="1253f-107">Když vytvoříte aplikaci funkce v hostování plán služby App Service, funkce aplikace můžete používat všechny funkce služby App Service.</span><span class="sxs-lookup"><span data-stu-id="1253f-107">When you create a function app in the App Service hosting plan, your function app can use all the features of App Service.</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="1253f-108">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="1253f-108">Create a function app</span></span>

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

<span data-ttu-id="1253f-109">Když vytvoříte aplikaci function app, zadejte platný **název aplikace**, který může obsahovat pouze písmena, číslice a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="1253f-109">When you create a function app, supply a valid **App name**, which can contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="1253f-110">Podtržítko (**_**) není povolené znak.</span><span class="sxs-lookup"><span data-stu-id="1253f-110">Underscore (**_**) is not an allowed character.</span></span>

<span data-ttu-id="1253f-111">Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="1253f-111">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="1253f-112">Váš název účtu úložiště musí být jedinečný v rámci Azure.</span><span class="sxs-lookup"><span data-stu-id="1253f-112">Your storage account name must be unique within Azure.</span></span> 

<span data-ttu-id="1253f-113">Po vytvoření aplikace funkce vytvořením jednotlivých funkcí na jeden nebo více různých jazyků.</span><span class="sxs-lookup"><span data-stu-id="1253f-113">After the function app is created, you can create individual functions in one or more different languages.</span></span> <span data-ttu-id="1253f-114">Vytvoření funkce [pomocí portálu](functions-create-first-azure-function.md#create-function), [průběžné nasazování](functions-continuous-deployment.md), nebo pomocí [odesílání spolu s FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span><span class="sxs-lookup"><span data-stu-id="1253f-114">Create functions [by using the portal](functions-create-first-azure-function.md#create-function), [continuous deployment](functions-continuous-deployment.md), or by [uploading with FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).</span></span>

## <a name="service-plans"></a><span data-ttu-id="1253f-115">Plány služeb</span><span class="sxs-lookup"><span data-stu-id="1253f-115">Service plans</span></span>

<span data-ttu-id="1253f-116">Azure Functions nabízí dva druhy různých služeb: plánování využívání a plán služby App Service.</span><span class="sxs-lookup"><span data-stu-id="1253f-116">Azure Functions has two different service plans: Consumption plan and App Service plan.</span></span> <span data-ttu-id="1253f-117">Plánu spotřeby automaticky přiděluje výpočetní výkon, pokud kód je spuštěný, měřítka na více systémů v případě potřeby zpracování zátěže, pak měřítka in a pokud kód není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1253f-117">The Consumption plan automatically allocates compute power when your code is running, scales-out as necessary to handle load, and then scales-in when code is not running.</span></span> <span data-ttu-id="1253f-118">Plán služby App Service poskytuje vaší funkce aplikace přístup k veškeré prostředky služby App Service.</span><span class="sxs-lookup"><span data-stu-id="1253f-118">The App Service plan gives your function app access to all the facilities of App Service.</span></span> <span data-ttu-id="1253f-119">Když je vytvořena funkce aplikace a nelze ji změnit, musíte zvolit plán služby.</span><span class="sxs-lookup"><span data-stu-id="1253f-119">You must choose your service plan when your function app is created, and it cannot currently be changed.</span></span> <span data-ttu-id="1253f-120">Další informace najdete v tématu [zvolte Azure Functions hostování plán](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="1253f-120">For more information, see [Choose an Azure Functions hosting plan](functions-scale.md).</span></span>

<span data-ttu-id="1253f-121">Pokud plánujete spouštět funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán s menším počtem jader.</span><span class="sxs-lookup"><span data-stu-id="1253f-121">If you are planning to run JavaScript functions on an App Service plan, you should choose a plan with fewer cores.</span></span> <span data-ttu-id="1253f-122">Další informace najdete v tématu [referenční dokumentace technologie JavaScript pro funkce](functions-reference-node.md#choose-single-core-app-service-plans).</span><span class="sxs-lookup"><span data-stu-id="1253f-122">For more information, see the [JavaScript reference for Functions](functions-reference-node.md#choose-single-core-app-service-plans).</span></span>

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a><span data-ttu-id="1253f-123">Požadavky na účet úložiště</span><span class="sxs-lookup"><span data-stu-id="1253f-123">Storage account requirements</span></span>

<span data-ttu-id="1253f-124">Když vytváříte aplikaci funkce ve službě App Service, musíte vytvořit nebo odkaz na účet pro obecné účely Azure Storage, který podporuje úložiště Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="1253f-124">When creating a function app in App Service, you must create or link to a general-purpose Azure Storage account that supports Blob, Queue, and Table storage.</span></span> <span data-ttu-id="1253f-125">Funkce interně používá úložiště pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce.</span><span class="sxs-lookup"><span data-stu-id="1253f-125">Internally, Functions uses Storage for operations such as managing triggers and logging function executions.</span></span> <span data-ttu-id="1253f-126">Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště Azure Premium Storage a účty úložiště pro obecné účely replikaci ZRS.</span><span class="sxs-lookup"><span data-stu-id="1253f-126">Some storage accounts do not support queues and tables, such as blob-only storage accounts, Azure Premium Storage, and general-purpose storage accounts with ZRS replication.</span></span> <span data-ttu-id="1253f-127">Tyto účty jsou filtrovány mimo v okně účtu úložiště při vytváření aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="1253f-127">These accounts are filtered out of from the Storage Account blade when creating a function app.</span></span>

>[!NOTE]
><span data-ttu-id="1253f-128">Při použití spotřeby hostování plán, funkce kód a vazba konfiguračních souborů jsou uložené v Azure File storage v účtu hlavní úložiště.</span><span class="sxs-lookup"><span data-stu-id="1253f-128">When using the Consumption hosting plan, your function code and binding configuration files are stored in Azure File storage in the main storage account.</span></span> <span data-ttu-id="1253f-129">Pokud odstraníte účet úložiště hlavní, tento obsah se odstraní a nelze jej obnovit.</span><span class="sxs-lookup"><span data-stu-id="1253f-129">When you delete the main storage account, this content is deleted and cannot be recovered.</span></span>

<span data-ttu-id="1253f-130">Další informace o typech účtu úložiště najdete v tématu [Představení služby Azure Storage](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span><span class="sxs-lookup"><span data-stu-id="1253f-130">To learn more about storage account types, see [Introducing the Azure Storage Services](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="1253f-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1253f-131">Next steps</span></span>

[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]



