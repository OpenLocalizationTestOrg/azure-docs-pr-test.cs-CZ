---
title: "Přidejte konektor Office 365 uživatelé v Logic Apps | Microsoft Docs"
description: "Přehled konektoru uživatelé služeb Office 365 s parametry rozhraní REST API"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 330f733440932a769eb0fe6031cd0d947a820080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-office-365-users-connector"></a><span data-ttu-id="cb393-103">Začínáme s konektorem uživatelé Office 365.</span><span class="sxs-lookup"><span data-stu-id="cb393-103">Get started with the Office 365 Users connector</span></span>
<span data-ttu-id="cb393-104">Připojte k Office 365 uživatelům profily, uživatelé vyhledávající a další.</span><span class="sxs-lookup"><span data-stu-id="cb393-104">Connect to Office 365 Users to get profiles, search users, and more.</span></span> <span data-ttu-id="cb393-105">Uživatelé služeb Office 365 můžete:</span><span class="sxs-lookup"><span data-stu-id="cb393-105">With Office 365 Users, you can:</span></span>

* <span data-ttu-id="cb393-106">Sestavení vaší firmy toku na základě dat, které můžete získat z uživatelé služeb Office 365.</span><span class="sxs-lookup"><span data-stu-id="cb393-106">Build your business flow based on the data you get from Office 365 Users.</span></span> 
* <span data-ttu-id="cb393-107">Použití akce, které získají přímé podřízené načíst profil uživatele správce a další.</span><span class="sxs-lookup"><span data-stu-id="cb393-107">Use actions that get direct reports, get a manager's user profile, and more.</span></span> <span data-ttu-id="cb393-108">Tyto akce se odpověď a pak proveďte výstup k dispozici pro další akce.</span><span class="sxs-lookup"><span data-stu-id="cb393-108">These actions get a response, and then make the output available for other actions.</span></span> <span data-ttu-id="cb393-109">Například získat přímé podřízené osoby a pak proveďte tyto informace a aktualizace databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="cb393-109">For example, get a person's direct reports, and then take this information and update a SQL Azure database.</span></span> 

<span data-ttu-id="cb393-110">Můžete začít s vytvářením aplikace logiky teď najdete v tématu [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="cb393-110">You can get started by creating a logic app now, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="create-a-connection-to-office-365-users"></a><span data-ttu-id="cb393-111">Umožňuje vytvořit připojení k uživatelé Office 365.</span><span class="sxs-lookup"><span data-stu-id="cb393-111">Create a connection to Office 365 Users</span></span>
<span data-ttu-id="cb393-112">Když přidáte tento konektor aplikace logiky, musíte přihlásit ke svému účtu Office 365 uživatelů a povolit logic apps, abyste připojení k vašemu účtu.</span><span class="sxs-lookup"><span data-stu-id="cb393-112">When you add this connector to your logic apps, you must sign-in to your Office 365 Users account and allow logic apps to connect to your account.</span></span>

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

<span data-ttu-id="cb393-113">Po vytvoření připojení, zadáte vlastnosti uživatelé služeb Office 365, jako je ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb393-113">After you create the connection, you enter the Office 365 Users properties, like the user ID.</span></span> <span data-ttu-id="cb393-114">**Odkazu k REST API** v tomto tématu popisuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cb393-114">The **REST API reference** in this topic describes these properties.</span></span>

## <a name="connector-specific-details"></a><span data-ttu-id="cb393-115">Podrobnosti o konkrétní konektor</span><span class="sxs-lookup"><span data-stu-id="cb393-115">Connector-specific details</span></span>

<span data-ttu-id="cb393-116">Zobrazit všechny aktivační události a akce definované v swagger a také zobrazit žádné limity v [connector – podrobnosti](/connectors/officeusers/).</span><span class="sxs-lookup"><span data-stu-id="cb393-116">View any triggers and actions defined in the swagger, and also see any limits in the [connector details](/connectors/officeusers/).</span></span>

## <a name="more-connectors"></a><span data-ttu-id="cb393-117">Více konektorů</span><span class="sxs-lookup"><span data-stu-id="cb393-117">More connectors</span></span>
<span data-ttu-id="cb393-118">Přejděte zpět [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="cb393-118">Go back to the [APIs list](apis-list.md).</span></span>