---
title: "Služba aaaSelf registrační portál pro spolupráci Azure Active Directory s B2B | Microsoft Docs"
description: "Spolupráce Azure Active Directory s B2B podporuje vaše vztahy povolením obchodní partnery tooselectively přístup k podnikovým aplikacím"
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
ms.openlocfilehash: c78920ecf812f7efc06a8b54b1fff834c32904f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a><span data-ttu-id="63c70-103">Samoobslužný portál pro registraci spolupráce Azure AD B2B</span><span class="sxs-lookup"><span data-stu-id="63c70-103">Self-service portal for Azure AD B2B collaboration sign-up</span></span>

<span data-ttu-id="63c70-104">Zákazníci můžete dělat mnohem s hello integrované funkce, které jsou k dispozici prostřednictvím našich správce IT [portál Azure](https://portal.azure.com) a na našem [Panel přístupu aplikace](https://myapps.microsoft.com) pro koncové uživatele.</span><span class="sxs-lookup"><span data-stu-id="63c70-104">Customers can do a lot with hello built-in features that are exposed through our IT admin [Azure portal](https://portal.azure.com) and our [Application Access Panel](https://myapps.microsoft.com) for end users.</span></span> <span data-ttu-id="63c70-105">Ale máme také informace, že podnikům nutnost pracovního postupu registrace hello toocustomize B2B uživatelé toofit potřebám své organizace.</span><span class="sxs-lookup"><span data-stu-id="63c70-105">But we are also aware that businesses need toocustomize hello onboarding workflow for B2B users toofit their organization’s needs.</span></span> <span data-ttu-id="63c70-106">Dělají to pomocí [našem rozhraní API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span><span class="sxs-lookup"><span data-stu-id="63c70-106">They can do that with [our API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span></span>

<span data-ttu-id="63c70-107">Diskuze s našich zákazníků vidíte, že jeden běžné potřebovat zvýšení až nad všemi ostatními.</span><span class="sxs-lookup"><span data-stu-id="63c70-107">In discussions with our customers, we see one common need rise up above all others.</span></span> <span data-ttu-id="63c70-108">Hello pozvání organizace nemusí vědět předem, který text hello, že jsou jednotlivé externími spolupracovníky, kteří potřebují přístup k prostředkům tootheir.</span><span class="sxs-lookup"><span data-stu-id="63c70-108">hello inviting organization may not know ahead of time who hello individual external collaborators are who need access tootheir resources.</span></span> <span data-ttu-id="63c70-109">Však chtěli způsob, jak pro uživatele z partnerských společností příliš zaregistrovat sami pomocí sady zásad tohoto hello pozvání ovládací prvky organizace.</span><span class="sxs-lookup"><span data-stu-id="63c70-109">They wanted a way for users from partner companies too sign themselves up with a set of policies that hello inviting organization controls.</span></span> <span data-ttu-id="63c70-110">Tento scénář je možné provádět prostřednictvím rozhraní API, takže jsme publikovaná projektu na Githubu, který se právě který: [ukázkový projekt Githubu](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="63c70-110">This scenario is possible through our APIs,  so we published a project on Github that did just that: [sample Github project](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

<span data-ttu-id="63c70-111">Naše Githubu projektu ukazuje, jak můžete použít rozhraní API organizace a poskytovat na základě zásad, samoobslužné funkce registrace pro jejich důvěryhodným partnerům s pravidla, která určují hello aplikace, které získají přístup k.</span><span class="sxs-lookup"><span data-stu-id="63c70-111">Our Github project demonstrates how organizations can use our APIs, and provide a policy-based, self-service sign-up capability for their trusted partners, with rules that determine hello apps they can access.</span></span> <span data-ttu-id="63c70-112">Uživatelům z partnerských můžete získat přístup tooresources, kdy je potřebují, bezpečně, bez nutnosti hello pozvání zaváděním toomanually organizace je.</span><span class="sxs-lookup"><span data-stu-id="63c70-112">Partner users can get access tooresources when they need them, securely, without requiring hello inviting organization toomanually onboard them.</span></span> <span data-ttu-id="63c70-113">Můžete snadno nasadit hello projektu do předplatného služby Azure podle svého výběru.</span><span class="sxs-lookup"><span data-stu-id="63c70-113">You can easily deploy hello project into an Azure subscription of your choice.</span></span>

## <a name="as-is-code"></a><span data-ttu-id="63c70-114">Jako-je kód</span><span class="sxs-lookup"><span data-stu-id="63c70-114">As-is code</span></span>

<span data-ttu-id="63c70-115">Mějte na paměti, že tento kód je k dispozici jako použití ukázkové toodemonstrate hello Azure Active Directory s B2B pozvání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="63c70-115">Remember that this code is made available as a sample toodemonstrate usage of hello Azure Active Directory B2B invitation API.</span></span> <span data-ttu-id="63c70-116">Je nutné přizpůsobit svým týmem vývojářů nebo partnera a by měl být zkontrolovány před nasazením v produkční scénář.</span><span class="sxs-lookup"><span data-stu-id="63c70-116">It should be customized by your dev team or a partner, and should be reviewed before being deployed in a production scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63c70-117">Další kroky</span><span class="sxs-lookup"><span data-stu-id="63c70-117">Next steps</span></span>

<span data-ttu-id="63c70-118">Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:</span><span class="sxs-lookup"><span data-stu-id="63c70-118">Browse our other articles on Azure AD B2B collaboration:</span></span>
* [<span data-ttu-id="63c70-119">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="63c70-119">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="63c70-120">Jak Azure Active Directory správci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="63c70-120">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="63c70-121">Jak informační pracovníci přidat uživatele spolupráce B2B?</span><span class="sxs-lookup"><span data-stu-id="63c70-121">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="63c70-122">elementy Hello hello e-mailová pozvánka pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="63c70-122">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="63c70-123">Uplatnění pozvánku spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="63c70-123">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="63c70-124">Licencování Azure AD s B2B spolupráce</span><span class="sxs-lookup"><span data-stu-id="63c70-124">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="63c70-125">Řešení potíží s spolupráce Azure Active Directory s B2B</span><span class="sxs-lookup"><span data-stu-id="63c70-125">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="63c70-126">Spolupráce Azure Active Directory s B2B nejčastější dotazy (FAQ)</span><span class="sxs-lookup"><span data-stu-id="63c70-126">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="63c70-127">Vícefaktorové ověřování pro uživatele pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="63c70-127">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="63c70-128">Přidání uživatelů spolupráce B2B bez Pozvánka</span><span class="sxs-lookup"><span data-stu-id="63c70-128">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="63c70-129">Rejstřík článků o správě aplikací ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63c70-129">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)