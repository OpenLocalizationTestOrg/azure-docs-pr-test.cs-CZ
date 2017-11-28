---
title: "aaaHow toogrant oprávnění tooa zákaznických aplikace | Microsoft Docs"
description: "Jak toogrant oprávnění tooyour zákaznických aplikace pomocí hello parametr adresy URL nebo portálu Azure AD"
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
ms.openlocfilehash: e43a105fff60fbf912bdf4f60260f86ee289328d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogrant-permissions-tooa-custom-developed-application"></a><span data-ttu-id="de325-103">Jak toogrant oprávnění tooa zákaznických aplikace</span><span class="sxs-lookup"><span data-stu-id="de325-103">How toogrant permissions tooa custom-developed application</span></span>

<span data-ttu-id="de325-104">Pokud chcete souhlas toogrant ho preventivně ve vaší aplikaci nebo jsou spuštěné došlo k chybě aplikace tooan nebyly dá souhlas, zkuste níže uvedeného postupu.</span><span class="sxs-lookup"><span data-stu-id="de325-104">If you want toogrant consent preemptively on your app or are running into an error that you have not consented tooan app, try these steps below.</span></span>

## <a name="how-tooperform-admin-consent-for-your-application"></a><span data-ttu-id="de325-105">Jak tooperform souhlas správce pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="de325-105">How tooperform Admin Consent for your application</span></span>

<span data-ttu-id="de325-106">Tato akce nemá vliv hello udělení souhlasu toohello aplikací pro všechny uživatele ve vaší organizaci.</span><span class="sxs-lookup"><span data-stu-id="de325-106">This has hello effect of granting consent toohello application for all users in your organization.</span></span>

1. <span data-ttu-id="de325-107">Přejděte toohello **registrace aplikace** jako **globálního správce**, pak vyberte aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="de325-107">Navigate toohello **App Registrations** blade as a **global administrator**, then select hello app.</span></span>

2. <span data-ttu-id="de325-108">Vyberte **požadovaných oprávnění**a nakonec klikněte na tlačítko hello **udělit oprávnění** tlačítko hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="de325-108">Select **Required Permissions**, and finally hit hello **Grant Permissions** button at hello top of hello blade.</span></span>

<span data-ttu-id="de325-109">Alternativně můžete vytvořit žádost o příliš*login.microsoftonline.com* s konfigurací vaší aplikace a připojte na *& výzva = správce\_souhlas*.</span><span class="sxs-lookup"><span data-stu-id="de325-109">Alternatively, you can construct a request too*login.microsoftonline.com* with your app configs and append on *&prompt=admin\_consent*.</span></span> <span data-ttu-id="de325-110">Po přihlášení pomocí přihlašovacích údajů správce, aplikace hello byl udělen souhlas pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="de325-110">After signing in with admin credentials, hello app has been granted consent for all users.</span></span>

## <a name="how-tooforce-user-consent-for-your-application"></a><span data-ttu-id="de325-111">Jak tooforce souhlas uživatele pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="de325-111">How tooforce User Consent for your application</span></span>

* <span data-ttu-id="de325-112">Připojit do žádosti o ověření *& výzva = souhlasu* vyžadující koncoví uživatelé tooconsent pokaždé, když ověření.</span><span class="sxs-lookup"><span data-stu-id="de325-112">Append onto auth requests *&prompt=consent* which require end users tooconsent each time they authenticate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de325-113">Další kroky</span><span class="sxs-lookup"><span data-stu-id="de325-113">Next steps</span></span>

[<span data-ttu-id="de325-114">TooAzureAD souhlasu a integrace aplikací</span><span class="sxs-lookup"><span data-stu-id="de325-114">Consent and Integrating Apps tooAzureAD</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[<span data-ttu-id="de325-115">Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace</span><span class="sxs-lookup"><span data-stu-id="de325-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="de325-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="de325-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
