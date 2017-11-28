---
title: "aaaRegister aplikace s koncovým bodem v2.0 hello Azure AD pomocí portálu hello | Microsoft Docs"
description: "Jak tooregister aplikace se společností Microsoft pro povolení přihlášení a přístup k Microsoft služby pomocí koncového bodu v2.0 hello"
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
ms.openlocfilehash: c56c98906656062435516e820cb318a04c03149c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooregister-an-app-with-hello-v20-endpoint"></a><span data-ttu-id="c7ed9-103">Jak tooregister aplikace s koncovým bodem v2.0 hello</span><span class="sxs-lookup"><span data-stu-id="c7ed9-103">How tooregister an app with hello v2.0 endpoint</span></span>
<span data-ttu-id="c7ed9-104">toobuild aplikaci, která přijímá MSA & Azure AD přihlášení, je nutné nejdříve tooregister aplikace se společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-104">toobuild an app that accepts both MSA & Azure AD sign-in, you'll first need tooregister an app with Microsoft.</span></span>  <span data-ttu-id="c7ed9-105">V tomto okamžiku nebude se moct toouse všechny existující aplikace, které jste uzavřeli s Azure AD nebo MSA – budete potřebovat toocreate brand nový.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-105">At this time, you won't be able toouse any existing apps you may have with Azure AD or MSA - you'll need toocreate a brand new one.</span></span>

> [!NOTE]
> <span data-ttu-id="c7ed9-106">Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-106">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="c7ed9-107">toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="c7ed9-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="visit-hello-microsoft-app-registration-portal"></a><span data-ttu-id="c7ed9-108">Navštívit portál pro registraci aplikace Microsoft hello</span><span class="sxs-lookup"><span data-stu-id="c7ed9-108">Visit hello Microsoft app registration portal</span></span>
<span data-ttu-id="c7ed9-109">Nejdůležitější - první návštěvě příliš[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span><span class="sxs-lookup"><span data-stu-id="c7ed9-109">First things first - navigate too[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).</span></span>  <span data-ttu-id="c7ed9-110">Toto je nový portál registrace aplikace, kde můžete spravovat aplikace Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-110">This is a new app registration portal where you can manage your Microsoft apps.</span></span>

<span data-ttu-id="c7ed9-111">Přihlaste se pomocí buď osobní nebo pracovní nebo školní účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-111">Sign in with either a personal or work or school Microsoft account.</span></span>  <span data-ttu-id="c7ed9-112">Pokud nemáte buď, zaregistrujte si nový osobní účet.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-112">If you don't have either, sign up for a new personal account.</span></span> <span data-ttu-id="c7ed9-113">Pokračujte, nebude trvat dlouho – budete čekáme sem.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-113">Go ahead, it won't take long - we'll wait here.</span></span>

<span data-ttu-id="c7ed9-114">Provést?</span><span class="sxs-lookup"><span data-stu-id="c7ed9-114">Done?</span></span> <span data-ttu-id="c7ed9-115">By měl nyní se díváte v seznamu aplikací Microsoftu, což je pravděpodobně prázdná.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-115">You should now be looking at your list of Microsoft apps, which is probably empty.</span></span>  <span data-ttu-id="c7ed9-116">Umožňuje změnit.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-116">Let's change that.</span></span>

<span data-ttu-id="c7ed9-117">Klikněte na tlačítko **přidat aplikaci**a pojmenujte ho.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-117">Click **Add an app**, and give it a name.</span></span>  <span data-ttu-id="c7ed9-118">portál Hello přiřadí aplikace globálně jedinečné Id aplikace, které budete používat později ve svém kódu.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-118">hello portal will assign your app a globally unique  Application Id that you'll use later in your code.</span></span>  <span data-ttu-id="c7ed9-119">Pokud vaše aplikace obsahuje serverovou komponentu potřebného přístupových tokenů pro volání rozhraní API (vezměte v úvahu: Office, Azure nebo vlastní webové rozhraní API), budete muset toocreate **tajný klíč aplikace** i zde.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-119">If your app includes a server-side component that needs access tokens for calling APIs (think: Office, Azure, or your own web API), you'll want toocreate an **Application Secret** here as well.</span></span>

<span data-ttu-id="c7ed9-120">Dál přidejte hello platformy, které vaše aplikace bude používat.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-120">Next, add hello Platforms that your app will use.</span></span>

* <span data-ttu-id="c7ed9-121">Pro webové aplikace, zadejte **identifikátor URI pro přesměrování** kde lze odesílat zprávy přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-121">For web based apps, provide a **Redirect URI** where sign-in messages can be sent.</span></span>
* <span data-ttu-id="c7ed9-122">Pro mobilní aplikace přesměruje poznamenejte hello výchozí identifikátor uri automaticky vytvoří za vás.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-122">For mobile apps, copy down hello default redirect uri automatically created for you.</span></span>

<span data-ttu-id="c7ed9-123">Volitelně můžete přizpůsobit hello vzhled a chování stránky přihlášení v hello profilu.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-123">Optionally, you can customize hello look and feel of your sign-in page in hello Profile section.</span></span>  <span data-ttu-id="c7ed9-124">Ujistěte se, že tooclick **Uložit** než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-124">Make sure tooclick **Save** before moving on.</span></span>

> [!NOTE]
> <span data-ttu-id="c7ed9-125">Při vytváření aplikace pomocí [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello aplikace se zaregistruje v klientovi domácí hello hello účtu používáte toosign hello portálu.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-125">When you create an application using [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello application will be registered in hello home tenant of hello account that you use toosign into hello portal.</span></span>  <span data-ttu-id="c7ed9-126">To znamená, že nemůže zaregistrovat aplikaci v klientovi služby Azure AD pomocí osobního účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-126">This means that you can not register an application in your Azure AD tenant using a personal Microsoft account.</span></span>  <span data-ttu-id="c7ed9-127">Nechcete-li explicitně tooregister aplikace v konkrétním klientovi, přihlaste se pomocí účtu, který původně vytvořil v něm.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-127">If you explicitly wish tooregister an application in a particular tenant, sign in with an account originally created in that tenant.</span></span>
> 
> 

## <a name="build-a-quick-start-app"></a><span data-ttu-id="c7ed9-128">Sestavení aplikace rychlý start</span><span class="sxs-lookup"><span data-stu-id="c7ed9-128">Build a quick start app</span></span>
<span data-ttu-id="c7ed9-129">Teď, když máte aplikaci Microsoft, můžete dokončit jeden z našich kurzů v2.0 rychlý start.</span><span class="sxs-lookup"><span data-stu-id="c7ed9-129">Now that you have a Microsoft app, you can complete one of our v2.0 quick start tutorials.</span></span>  <span data-ttu-id="c7ed9-130">Zde je několik doporučení:</span><span class="sxs-lookup"><span data-stu-id="c7ed9-130">Here are a few recommendations:</span></span>

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

