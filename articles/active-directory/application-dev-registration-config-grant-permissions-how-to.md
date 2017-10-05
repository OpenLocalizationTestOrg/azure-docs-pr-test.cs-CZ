---
title: "Postup udělení oprávnění k aplikaci zákaznických | Microsoft Docs"
description: "Postup udělení oprávnění pro aplikace vyvinuté vlastní pomocí portálu Azure AD nebo parametr adresy URL"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 336b945929f80e1a566f7cf71b40fd799a98c12d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a><span data-ttu-id="db384-103">Postup udělení oprávnění k aplikaci zákaznických</span><span class="sxs-lookup"><span data-stu-id="db384-103">How to grant permissions to a custom-developed application</span></span>

<span data-ttu-id="db384-104">Chcete-li k udělení souhlasu ho preventivně ve vaší aplikaci nebo jsou spuštěné aplikace došlo k chybě, které nebyly souhlas, zkuste níže uvedeného postupu.</span><span class="sxs-lookup"><span data-stu-id="db384-104">If you want to grant consent preemptively on your app or are running into an error that you have not consented to an app, try these steps below.</span></span>

## <a name="how-to-perform-admin-consent-for-your-application"></a><span data-ttu-id="db384-105">Jak provádět souhlas správce pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="db384-105">How to perform Admin Consent for your application</span></span>

<span data-ttu-id="db384-106">To má za následek udělení souhlasu do aplikace pro všechny uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="db384-106">This has the effect of granting consent to the application for all users in your organization.</span></span>

1. <span data-ttu-id="db384-107">Přejděte na **registrace aplikace** jako **globálního správce**, vyberte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db384-107">Navigate to the **App Registrations** blade as a **global administrator**, then select the app.</span></span>

2. <span data-ttu-id="db384-108">Vyberte **požadovaných oprávnění**a nakonec klikněte **udělit oprávnění** tlačítka v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="db384-108">Select **Required Permissions**, and finally hit the **Grant Permissions** button at the top of the blade.</span></span>

<span data-ttu-id="db384-109">Alternativně můžete vytvořit žádost o *login.microsoftonline.com* s konfigurací vaší aplikace a připojte na *& výzva = správce\_souhlas*.</span><span class="sxs-lookup"><span data-stu-id="db384-109">Alternatively, you can construct a request to *login.microsoftonline.com* with your app configs and append on *&prompt=admin\_consent*.</span></span> <span data-ttu-id="db384-110">Po přihlášení pomocí přihlašovacích údajů správce, aplikace byl udělen souhlas pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="db384-110">After signing in with admin credentials, the app has been granted consent for all users.</span></span>

## <a name="how-to-force-user-consent-for-your-application"></a><span data-ttu-id="db384-111">Jak vynutit souhlas uživatele pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="db384-111">How to force User Consent for your application</span></span>

* <span data-ttu-id="db384-112">Připojit do žádosti o ověření *& výzva = souhlasu* vyžadující koncovým uživatelům souhlas pokaždé, když ověření.</span><span class="sxs-lookup"><span data-stu-id="db384-112">Append onto auth requests *&prompt=consent* which require end users to consent each time they authenticate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db384-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db384-113">Next steps</span></span>

[<span data-ttu-id="db384-114">Souhlasu a integrace aplikací AzureAD</span><span class="sxs-lookup"><span data-stu-id="db384-114">Consent and Integrating Apps to AzureAD</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[<span data-ttu-id="db384-115">Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace</span><span class="sxs-lookup"><span data-stu-id="db384-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="db384-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="db384-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
