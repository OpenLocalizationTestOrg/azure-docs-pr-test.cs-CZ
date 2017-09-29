---
title: "Testovací soubor Sipi | Microsoft Docs"
description: "Testovací soubor ke kontrole ReadyForTest závislosti"
services: active-directory-b2c
documentationcenter: 
author: Sipi
manager: Sipi
editor: Sipi
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f68
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: Sipi
ms.openlocfilehash: 871d58818dcbaee5f7a5f07c19e2297ec6459a6f
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="sipi-test-file"></a><span data-ttu-id="b04fe-103">Testovací soubor Sipi</span><span class="sxs-lookup"><span data-stu-id="b04fe-103">Sipi test file</span></span>

<span data-ttu-id="b04fe-104">Tento rychlý start vám pomůže zaregistrovat aplikaci v tenantovi Microsoft Azure Active Directory (Azure AD) B2C během několika minut.</span><span class="sxs-lookup"><span data-stu-id="b04fe-104">This Quickstart helps you register an application in a Microsoft Azure Active Directory (Azure AD) B2C tenant in a few minutes.</span></span> <span data-ttu-id="b04fe-105">Jakmile budete hotovi, vaše aplikace bude zaregistrovaná pro použití v tenantovi Azure B2C.</span><span class="sxs-lookup"><span data-stu-id="b04fe-105">When you're finished, your application is registered for use in the Azure B2C tenant.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b04fe-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b04fe-106">Prerequisites</span></span>

<span data-ttu-id="b04fe-107">Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C.</span><span class="sxs-lookup"><span data-stu-id="b04fe-107">To build an application that accepts consumer sign-up and sign-in, you first need to register the application with an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="b04fe-108">Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b04fe-108">Get your own tenant by using the steps outlined in [Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>

<span data-ttu-id="b04fe-109">Aplikace vytvořené z okna Azure AD B2C na webu Azure Portal se musí spravovat ze stejného místa.</span><span class="sxs-lookup"><span data-stu-id="b04fe-109">Applications created from the Azure AD B2C blade in the Azure portal must be managed from the same location.</span></span> <span data-ttu-id="b04fe-110">Pokud upravíte aplikace B2C pomocí PowerShellu nebo jiného portálu, stanou se nepodporované a nebudou s Azure AD B2C pracovat.</span><span class="sxs-lookup"><span data-stu-id="b04fe-110">If you edit the B2C applications using PowerShell or another portal, they become unsupported and do not work with Azure AD B2C.</span></span> <span data-ttu-id="b04fe-111">Podrobnosti najdete v části věnující se [chybným aplikacím](#faulted-apps).</span><span class="sxs-lookup"><span data-stu-id="b04fe-111">See details in the [faulted apps](#faulted-apps) section.</span></span> 

## <a name="navigate-to-b2c-settings"></a><span data-ttu-id="b04fe-112">Přechod do nastavení B2C</span><span class="sxs-lookup"><span data-stu-id="b04fe-112">Navigate to B2C settings</span></span>

<span data-ttu-id="b04fe-113">Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce tenanta B2C.</span><span class="sxs-lookup"><span data-stu-id="b04fe-113">Log in to the [Azure portal](https://portal.azure.com/) as the Global Administrator of the B2C tenant.</span></span> 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

<span data-ttu-id="b04fe-114">Další postup zvolte podle typu aplikace, kterou registrujete:</span><span class="sxs-lookup"><span data-stu-id="b04fe-114">Choose next steps based on the application type you are registering:</span></span>
