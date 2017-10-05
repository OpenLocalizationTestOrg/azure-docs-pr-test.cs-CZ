---
title: "Registrační portál samoobslužné služby pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce B2B ve službě Azure Active Directory podporuje vaše vztahy s ostatními společnostmi tím, že vašim obchodním partnerům umožní selektivní přístup ke podnikovým aplikacím"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a><span data-ttu-id="73436-103">Samoobslužný portál pro registraci spolupráce Azure AD B2B</span><span class="sxs-lookup"><span data-stu-id="73436-103">Self-service portal for Azure AD B2B collaboration sign-up</span></span>

<span data-ttu-id="73436-104">Zákazníci můžete dělat mnohem s integrované funkce, které jsou k dispozici prostřednictvím našich správce IT [portál Azure](https://portal.azure.com) a na našem [Panel přístupu aplikace](https://myapps.microsoft.com) pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="73436-104">Customers can do a lot with the built-in features that are exposed through our IT admin [Azure portal](https://portal.azure.com) and our [Application Access Panel](https://myapps.microsoft.com) for end users.</span></span> <span data-ttu-id="73436-105">Ale máme také informace, že podnikům potřeba přizpůsobení pracovního postupu registrace pro uživatele B2B podle potřeb jejich organizace.</span><span class="sxs-lookup"><span data-stu-id="73436-105">But we are also aware that businesses need to customize the onboarding workflow for B2B users to fit their organization’s needs.</span></span> <span data-ttu-id="73436-106">Dělají to pomocí [našem rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span><span class="sxs-lookup"><span data-stu-id="73436-106">They can do that with [our API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span></span>

<span data-ttu-id="73436-107">Diskuze s našich zákazníků vidíte, že jeden běžné potřebovat zvýšení až nad všemi ostatními.</span><span class="sxs-lookup"><span data-stu-id="73436-107">In discussions with our customers, we see one common need rise up above all others.</span></span> <span data-ttu-id="73436-108">Pozváním organizace nemusí vědět předem, kdo jsou jednotlivé externími spolupracovníky kteří potřebují přístup ke svým prostředkům.</span><span class="sxs-lookup"><span data-stu-id="73436-108">The inviting organization may not know ahead of time who the individual external collaborators are who need access to their resources.</span></span> <span data-ttu-id="73436-109">Způsob, jak se chtěli pro uživatele z partnerských společností sami zaregistrovat pomocí sady zásad, které řídí pozváním organizace.</span><span class="sxs-lookup"><span data-stu-id="73436-109">They wanted a way for users from partner companies to  sign themselves up with a set of policies that the inviting organization controls.</span></span> <span data-ttu-id="73436-110">Tento scénář je možné provádět prostřednictvím rozhraní API, takže jsme publikovaná projektu na Githubu, který se právě který: [ukázkový projekt Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="73436-110">This scenario is possible through our APIs,  so we published a project on Github that did just that: [sample Github project](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

<span data-ttu-id="73436-111">Naše Githubu projektu ukazuje, jak můžete použít rozhraní API organizace a poskytovat na základě zásad, samoobslužné funkce registrace pro jejich důvěryhodným partnerům, pravidla, která určit aplikace, které získají přístup k.</span><span class="sxs-lookup"><span data-stu-id="73436-111">Our Github project demonstrates how organizations can use our APIs, and provide a policy-based, self-service sign-up capability for their trusted partners, with rules that determine the apps they can access.</span></span> <span data-ttu-id="73436-112">Uživatelé partnera můžete získat přístup k prostředkům, kdy je potřebují, bezpečně, bez nutnosti jejich pozváním organizace ručně zařadit do provozu.</span><span class="sxs-lookup"><span data-stu-id="73436-112">Partner users can get access to resources when they need them, securely, without requiring the inviting organization to manually onboard them.</span></span> <span data-ttu-id="73436-113">Můžete snadno nasaďte projekt do předplatného služby Azure podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="73436-113">You can easily deploy the project into an Azure subscription of your choice.</span></span>

## <a name="as-is-code"></a><span data-ttu-id="73436-114">Jako-je kód</span><span class="sxs-lookup"><span data-stu-id="73436-114">As-is code</span></span>

<span data-ttu-id="73436-115">Mějte na paměti, že tento kód je k dispozici jako ukázka k předvedení využití pozvánku Azure Active Directory s B2B rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73436-115">Remember that this code is made available as a sample to demonstrate usage of the Azure Active Directory B2B invitation API.</span></span> <span data-ttu-id="73436-116">Je nutné přizpůsobit svým týmem vývojářů nebo partnera a by měl být zkontrolovány před nasazením v produkční scénář.</span><span class="sxs-lookup"><span data-stu-id="73436-116">It should be customized by your dev team or a partner, and should be reviewed before being deployed in a production scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73436-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="73436-117">Next steps</span></span>

<span data-ttu-id="73436-118">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="73436-118">Browse our other articles on Azure AD B2B collaboration:</span></span>
* [<span data-ttu-id="73436-119">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="73436-119">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="73436-120">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="73436-120">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="73436-121">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="73436-121">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="73436-122">Elementy e-mail pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="73436-122">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="73436-123">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="73436-123">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="73436-124">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="73436-124">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="73436-125">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="73436-125">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="73436-126">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="73436-126">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="73436-127">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="73436-127">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="73436-128">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="73436-128">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="73436-129">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73436-129">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)