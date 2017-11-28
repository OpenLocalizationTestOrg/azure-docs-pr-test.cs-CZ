---
title: "Azure Active Directory B2C: Zakázat ověření e-mailu během registrace příjemce | Microsoft Docs"
description: "Téma ukázka, jak toodisable e-mailu ověření během registrace v Azure Active Directory B2C příjemce"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.openlocfilehash: a8a42eddcb577725f04d70e1b1ebbebf10b5937c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a><span data-ttu-id="1af70-103">Azure Active Directory B2C: Zakázání e-mailu ověření během registrace příjemce</span><span class="sxs-lookup"><span data-stu-id="1af70-103">Azure Active Directory B2C: Disable email verification during consumer sign-up</span></span>
<span data-ttu-id="1af70-104">Když je povolené, poskytuje Azure Active Directory (Azure AD) B2C a příjemce hello toosign možnost pro aplikace pomocí e-mailovou adresu a vytvoření místního účtu.</span><span class="sxs-lookup"><span data-stu-id="1af70-104">When enabled, Azure Active Directory (Azure AD) B2C gives a consumer hello ability toosign up for applications by providing an email address and creating a local account.</span></span> <span data-ttu-id="1af70-105">Azure AD B2C zajišťuje platné e-mailové adresy, tím, že příjemci tooverify je během procesu registrace hello.</span><span class="sxs-lookup"><span data-stu-id="1af70-105">Azure AD B2C ensures valid email addresses by requiring consumers tooverify them during hello sign-up process.</span></span> <span data-ttu-id="1af70-106">Zabrání také škodlivý automatizovaného procesu z generování falešných účty pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="1af70-106">It also prevents a malicious automated process from generating fake accounts for hello applications.</span></span>

<span data-ttu-id="1af70-107">Někteří vývojáři aplikace raději tooskip e-mailu ověření během procesu registrace hello a místo toho mít příjemce ověřit e-mailovou adresu hello později.</span><span class="sxs-lookup"><span data-stu-id="1af70-107">Some application developers prefer tooskip email verification during hello sign-up process and instead have consumers verify hello email address later.</span></span> <span data-ttu-id="1af70-108">toosupport to Azure AD B2C můžete být nakonfigurované toodisable ověření e-mailu.</span><span class="sxs-lookup"><span data-stu-id="1af70-108">toosupport this, Azure AD B2C can be configured toodisable email verification.</span></span> <span data-ttu-id="1af70-109">To vytvoří hladší procesu registrace a poskytuje vývojářům hello flexibilitu toodifferentiate hello příjemci, které ověření e-mailové adresy z těchto příjemci, které ještě nebyly.</span><span class="sxs-lookup"><span data-stu-id="1af70-109">Doing so creates a smoother sign-up process and gives developers hello flexibility toodifferentiate hello consumers that have verified their email address from those consumers that have not.</span></span>

<span data-ttu-id="1af70-110">Zásady registrace mají ve výchozím nastavení zapnutá ověření e-mailu.</span><span class="sxs-lookup"><span data-stu-id="1af70-110">By default, sign-up policies have email verification turned on.</span></span> <span data-ttu-id="1af70-111">Použití hello následující kroky tooturn ho vypnout:</span><span class="sxs-lookup"><span data-stu-id="1af70-111">Use hello following steps tooturn it off:</span></span>

1. <span data-ttu-id="1af70-112">[Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="1af70-112">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="1af70-113">Klikněte na tlačítko **registrace zásady** nebo **zásady registrace nebo přihlášení** v závislosti na tom, co jste nakonfigurovali pro registraci.</span><span class="sxs-lookup"><span data-stu-id="1af70-113">Click **Sign-up policies** or **Sign-up or sign-in policies** depending on what you configured for sign-up.</span></span>
3. <span data-ttu-id="1af70-114">Klikněte na vaše zásady (například "B2C_1_SiUp") tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="1af70-114">Click your policy (for example, "B2C_1_SiUp") tooopen it.</span></span> <span data-ttu-id="1af70-115">Klikněte na tlačítko **upravit** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="1af70-115">Click **Edit** at hello top of hello blade.</span></span>
4. <span data-ttu-id="1af70-116">Klikněte na tlačítko **přizpůsobení uživatelského rozhraní stránky**.</span><span class="sxs-lookup"><span data-stu-id="1af70-116">Click **Page UI Customization**.</span></span>
5. <span data-ttu-id="1af70-117">Klikněte na tlačítko **stránku pro přihlášení místní účet**.</span><span class="sxs-lookup"><span data-stu-id="1af70-117">Click **Local account sign-up page**.</span></span>
6. <span data-ttu-id="1af70-118">Klikněte na tlačítko **e-mailovou adresu** v hello **název** sloupce pod hello **atributy registrace** části.</span><span class="sxs-lookup"><span data-stu-id="1af70-118">Click **Email Address** in hello **Name** column under hello **Sign-up attributes** section.</span></span>
7. <span data-ttu-id="1af70-119">Přepnutí hello **vyžadovat ověření** možnost příliš**ne**.</span><span class="sxs-lookup"><span data-stu-id="1af70-119">Toggle hello **Require verification** option too**No**.</span></span>
8. <span data-ttu-id="1af70-120">Klikněte na tlačítko **OK** dolnímu hello, dokud se nedostanete hello **upravit zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="1af70-120">Click **OK** at hello bottom until you reach hello **Edit policy** blade.</span></span>
9. <span data-ttu-id="1af70-121">Klikněte na tlačítko **Uložit** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="1af70-121">Click **Save** at hello top of hello blade.</span></span> <span data-ttu-id="1af70-122">Hotovo!</span><span class="sxs-lookup"><span data-stu-id="1af70-122">You're done!</span></span>

> [!NOTE]
> <span data-ttu-id="1af70-123">Zakázání e-mailu ověření v procesu registrace hello může způsobit, že toospam.</span><span class="sxs-lookup"><span data-stu-id="1af70-123">Disabling email verification in hello sign-up process may lead toospam.</span></span> <span data-ttu-id="1af70-124">Pokud zakážete hello výchozí nastavení, doporučujeme přidání vlastního ověřovacího systému.</span><span class="sxs-lookup"><span data-stu-id="1af70-124">If you disable hello default one, we recommend adding your own verification system.</span></span>
> 
> 

<span data-ttu-id="1af70-125">Snažíme se vždy otevřete toofeedback a návrhy!</span><span class="sxs-lookup"><span data-stu-id="1af70-125">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="1af70-126">Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="1af70-126">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="1af70-127">Pro žádosti o funkce, přidejte je příliš[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="1af70-127">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
