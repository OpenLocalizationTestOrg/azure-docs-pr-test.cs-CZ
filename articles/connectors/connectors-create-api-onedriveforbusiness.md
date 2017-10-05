---
title: OneDrive pro firmy | Microsoft Docs
description: "Vytvoření aplikace logiky službou Azure App service. Připojte k Onedrivu pro firmy a správa souborů. Můžete provádět různé akce, jako je například nahrávání, aktualizace, získání a odstranění na soubory."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: cf9484e9-7a20-4de0-93c8-0fa132221f2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 783d6a640d9626508bcabc5f991dc5b6fc22eaf4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-onedrive-for-business-connector"></a><span data-ttu-id="c1cfd-105">Začínáme s Onedrivu pro firmy konektoru</span><span class="sxs-lookup"><span data-stu-id="c1cfd-105">Get started with the OneDrive for Business connector</span></span>
<span data-ttu-id="c1cfd-106">Připojte k Onedrivu pro firmy a správa souborů.</span><span class="sxs-lookup"><span data-stu-id="c1cfd-106">Connect to OneDrive for Business to manage your files.</span></span> <span data-ttu-id="c1cfd-107">Můžete provádět různé akce, jako je například nahrávání, aktualizace, získání a odstranění na soubory.</span><span class="sxs-lookup"><span data-stu-id="c1cfd-107">You can perform various actions such as upload, update, get, and delete on files.</span></span>

<span data-ttu-id="c1cfd-108">Můžete začít s vytvářením aplikace logiky teď najdete v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="c1cfd-108">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-onedrive-for-business"></a><span data-ttu-id="c1cfd-109">Umožňuje vytvořit připojení k Onedrivu pro firmy</span><span class="sxs-lookup"><span data-stu-id="c1cfd-109">Create a connection to OneDrive for Business</span></span>
<span data-ttu-id="c1cfd-110">K vytvoření aplikace logiky s Onedrivem pro firmy, musíte nejdřív vytvořit **připojení** pak zadejte podrobnosti pro následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="c1cfd-110">To create Logic apps with OneDrive for Business, you must first create a **connection** then provide the details for the following properties:</span></span>

| <span data-ttu-id="c1cfd-111">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="c1cfd-111">Property</span></span> | <span data-ttu-id="c1cfd-112">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="c1cfd-112">Required</span></span> | <span data-ttu-id="c1cfd-113">Popis</span><span class="sxs-lookup"><span data-stu-id="c1cfd-113">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c1cfd-114">Token</span><span class="sxs-lookup"><span data-stu-id="c1cfd-114">Token</span></span> |<span data-ttu-id="c1cfd-115">Ano</span><span class="sxs-lookup"><span data-stu-id="c1cfd-115">Yes</span></span> |<span data-ttu-id="c1cfd-116">Zadejte OneDrive pro firmy pověření</span><span class="sxs-lookup"><span data-stu-id="c1cfd-116">Provide OneDrive for Business Credentials</span></span> |

<span data-ttu-id="c1cfd-117">Po vytvoření připojení, můžete ke spouštění akcí a naslouchat aktivační události popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="c1cfd-117">After you create the connection, you can use it to execute the actions and listen for the triggers described in this article.</span></span>

> [!INCLUDE [Steps to create a connection to OneDrive for Business](../../includes/connectors-create-api-onedriveforbusiness.md)]
> 

## <a name="connector-specific-details"></a><span data-ttu-id="c1cfd-118">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="c1cfd-118">Connector-specific details</span></span>

<span data-ttu-id="c1cfd-119">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/onedriveforbusinessconnector/).</span><span class="sxs-lookup"><span data-stu-id="c1cfd-119">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/onedriveforbusinessconnector/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="c1cfd-120">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="c1cfd-120">More connectors</span></span>
<span data-ttu-id="c1cfd-121">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="c1cfd-121">Go back to the [APIs list](apis-list.md).</span></span>