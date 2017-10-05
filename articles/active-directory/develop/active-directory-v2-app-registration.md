---
title: "Zaregistrovat aplikaci s koncovým bodem v2.0 Azure AD pomocí portálu | Microsoft Docs"
description: "Postup registrace aplikace se společností Microsoft pro povolení přihlášení a přístup ke službám Microsoft pomocí koncového bodu v2.0"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: e6202aa8665c906382666fe08a561421e50e0a8d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a><span data-ttu-id="ffad0-103">Postup registrace aplikace s koncovým bodem v2.0</span><span class="sxs-lookup"><span data-stu-id="ffad0-103">How to register an app with the v2.0 endpoint</span></span>
<span data-ttu-id="ffad0-104">Vytvořit aplikaci, která přijímá MSA & Azure AD přihlášení, nejprve musíte zaregistrovat aplikaci se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ffad0-104">To build an app that accepts both MSA & Azure AD sign-in, you'll first need to register an app with Microsoft.</span></span>  <span data-ttu-id="ffad0-105">V tomto okamžiku nebudete moci používat všechny existující aplikace, které jste uzavřeli s Azure AD nebo MSA - budete muset vytvořit nové.</span><span class="sxs-lookup"><span data-stu-id="ffad0-105">At this time, you won't be able to use any existing apps you may have with Azure AD or MSA - you'll need to create a brand new one.</span></span>

> [!NOTE]
> <span data-ttu-id="ffad0-106">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0.</span><span class="sxs-lookup"><span data-stu-id="ffad0-106">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="ffad0-107">Pokud chcete zjistit, pokud byste měli používat koncový bod v2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="ffad0-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="visit-the-microsoft-app-registration-portal"></a><span data-ttu-id="ffad0-108">Přejděte na portál pro registraci aplikace Microsoft</span><span class="sxs-lookup"><span data-stu-id="ffad0-108">Visit the Microsoft app registration portal</span></span>
<span data-ttu-id="ffad0-109">Nejdůležitější - první návštěvě [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="ffad0-109">First things first - navigate to [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="ffad0-110">Toto je nový portál registrace aplikace, kde můžete spravovat aplikace Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ffad0-110">This is a new app registration portal where you can manage your Microsoft apps.</span></span>

<span data-ttu-id="ffad0-111">Přihlaste se pomocí buď osobní nebo pracovní nebo školní účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ffad0-111">Sign in with either a personal or work or school Microsoft account.</span></span>  <span data-ttu-id="ffad0-112">Pokud nemáte buď, zaregistrujte si nový osobní účet.</span><span class="sxs-lookup"><span data-stu-id="ffad0-112">If you don't have either, sign up for a new personal account.</span></span> <span data-ttu-id="ffad0-113">Pokračujte, nebude trvat dlouho – budete čekáme sem.</span><span class="sxs-lookup"><span data-stu-id="ffad0-113">Go ahead, it won't take long - we'll wait here.</span></span>

<span data-ttu-id="ffad0-114">Provést?</span><span class="sxs-lookup"><span data-stu-id="ffad0-114">Done?</span></span> <span data-ttu-id="ffad0-115">By měl nyní se díváte v seznamu aplikací Microsoftu, což je pravděpodobně prázdná.</span><span class="sxs-lookup"><span data-stu-id="ffad0-115">You should now be looking at your list of Microsoft apps, which is probably empty.</span></span>  <span data-ttu-id="ffad0-116">Umožňuje změnit.</span><span class="sxs-lookup"><span data-stu-id="ffad0-116">Let's change that.</span></span>

<span data-ttu-id="ffad0-117">Klikněte na tlačítko **přidat aplikaci**a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="ffad0-117">Click **Add an app**, and give it a name.</span></span>  <span data-ttu-id="ffad0-118">Na portálu přiřadí globálně jedinečné Id aplikace, které budete používat později v kódu aplikace.</span><span class="sxs-lookup"><span data-stu-id="ffad0-118">The portal will assign your app a globally unique  Application Id that you'll use later in your code.</span></span>  <span data-ttu-id="ffad0-119">Pokud vaše aplikace obsahuje serverovou komponentu potřebného přístupových tokenů pro volání rozhraní API (vezměte v úvahu: Office, Azure nebo vlastní webové rozhraní API), budete chtít vytvořit **tajný klíč aplikace** i zde.</span><span class="sxs-lookup"><span data-stu-id="ffad0-119">If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want to create an **Application Secret** here as well.</span></span>

<span data-ttu-id="ffad0-120">Dál přidejte platformy, které vaše aplikace bude používat.</span><span class="sxs-lookup"><span data-stu-id="ffad0-120">Next, add the Platforms that your app will use.</span></span>

* <span data-ttu-id="ffad0-121">Pro webové aplikace, zadejte **identifikátor URI pro přesměrování** kde lze odesílat zprávy přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ffad0-121">For web based apps, provide a **Redirect URI** where sign-in messages can be sent.</span></span>
* <span data-ttu-id="ffad0-122">Identifikátor uri je automaticky vytvořen pro přesměrování pro mobilní aplikace, poznamenejte si výchozí.</span><span class="sxs-lookup"><span data-stu-id="ffad0-122">For mobile apps, copy down the default redirect uri automatically created for you.</span></span>

<span data-ttu-id="ffad0-123">Volitelně můžete přizpůsobit vzhled a chování stránky přihlášení v části profilu.</span><span class="sxs-lookup"><span data-stu-id="ffad0-123">Optionally, you can customize the look and feel of your sign-in page in the Profile section.</span></span>  <span data-ttu-id="ffad0-124">Ujistěte se, klikněte na **Uložit** než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ffad0-124">Make sure to click **Save** before moving on.</span></span>

> [!NOTE]
> <span data-ttu-id="ffad0-125">Při vytváření aplikace pomocí [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), aplikace bude zaregistrována v domácí klientovi účtu, který používáte k přihlášení k portálu.</span><span class="sxs-lookup"><span data-stu-id="ffad0-125">When you create an application using [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), the application will be registered in the home tenant of the account that you use to sign into the portal.</span></span>  <span data-ttu-id="ffad0-126">To znamená, že nemůže zaregistrovat aplikaci v klientovi služby Azure AD pomocí osobního účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ffad0-126">This means that you can not register an application in your Azure AD tenant using a personal Microsoft account.</span></span>  <span data-ttu-id="ffad0-127">Pokud chcete explicitně zaregistrovat aplikaci v konkrétní klienta, přihlaste se pomocí účtu, který původně vytvořil v něm.</span><span class="sxs-lookup"><span data-stu-id="ffad0-127">If you explicitly wish to register an application in a particular tenant, sign in with an account originally created in that tenant.</span></span>
> 
> 

## <a name="build-a-quick-start-app"></a><span data-ttu-id="ffad0-128">Sestavení aplikace rychlý start</span><span class="sxs-lookup"><span data-stu-id="ffad0-128">Build a quick start app</span></span>
<span data-ttu-id="ffad0-129">Teď, když máte aplikaci Microsoft, můžete dokončit jeden z našich kurzů v2.0 rychlý start.</span><span class="sxs-lookup"><span data-stu-id="ffad0-129">Now that you have a Microsoft app, you can complete one of our v2.0 quick start tutorials.</span></span>  <span data-ttu-id="ffad0-130">Zde je několik doporučení:</span><span class="sxs-lookup"><span data-stu-id="ffad0-130">Here are a few recommendations:</span></span>

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

