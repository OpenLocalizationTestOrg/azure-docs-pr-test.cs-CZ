---
title: "Azure Active Directory B2C: Zakázat ověření e-mailu během registrace příjemce | Microsoft Docs"
description: "Téma, který ukazuje, jak zakázat ověření e-mailu během registrace v Azure Active Directory B2C příjemce"
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
ms.openlocfilehash: d8e44a8aade60d21734477d60bccc2bd5194436e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a><span data-ttu-id="88367-103">Azure Active Directory B2C: Zakázání e-mailu ověření během registrace příjemce</span><span class="sxs-lookup"><span data-stu-id="88367-103">Azure Active Directory B2C: Disable email verification during consumer sign-up</span></span>
<span data-ttu-id="88367-104">Když je povolené, Azure Active Directory (Azure AD) B2C dává možnost zaregistrovat pro aplikace pomocí e-mailovou adresu a vytvoření místního účtu příjemce.</span><span class="sxs-lookup"><span data-stu-id="88367-104">When enabled, Azure Active Directory (Azure AD) B2C gives a consumer the ability to sign up for applications by providing an email address and creating a local account.</span></span> <span data-ttu-id="88367-105">Azure AD B2C zajišťuje platné e-mailové adresy, tím, že příjemci k ověření je během procesu registrace.</span><span class="sxs-lookup"><span data-stu-id="88367-105">Azure AD B2C ensures valid email addresses by requiring consumers to verify them during the sign-up process.</span></span> <span data-ttu-id="88367-106">Zabrání také škodlivý automatizovaného procesu z generování falešných účty pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="88367-106">It also prevents a malicious automated process from generating fake accounts for the applications.</span></span>

<span data-ttu-id="88367-107">Někteří vývojáři aplikace přednost přeskočit ověření e-mailu během procesu registrace a místo toho mít příjemce e-mailovou adresu ověřte později.</span><span class="sxs-lookup"><span data-stu-id="88367-107">Some application developers prefer to skip email verification during the sign-up process and instead have consumers verify the email address later.</span></span> <span data-ttu-id="88367-108">Za tímto účelem lze nakonfigurovat Azure AD B2C zakázat ověření e-mailu.</span><span class="sxs-lookup"><span data-stu-id="88367-108">To support this, Azure AD B2C can be configured to disable email verification.</span></span> <span data-ttu-id="88367-109">To vytvoří hladší procesu registrace a poskytuje vývojářům možnost odlišit od příjemce, které ověření e-mailové adresy z těchto příjemci, které ještě nebyly.</span><span class="sxs-lookup"><span data-stu-id="88367-109">Doing so creates a smoother sign-up process and gives developers the flexibility to differentiate the consumers that have verified their email address from those consumers that have not.</span></span>

<span data-ttu-id="88367-110">Zásady registrace mají ve výchozím nastavení zapnutá ověření e-mailu.</span><span class="sxs-lookup"><span data-stu-id="88367-110">By default, sign-up policies have email verification turned on.</span></span> <span data-ttu-id="88367-111">Chcete-li vypnout pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="88367-111">Use the following steps to turn it off:</span></span>

1. <span data-ttu-id="88367-112">[Postupujte podle těchto kroků přejděte do okna s funkcemi B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="88367-112">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="88367-113">Klikněte na tlačítko **registrace zásady** nebo **zásady registrace nebo přihlášení** v závislosti na tom, co jste nakonfigurovali pro registraci.</span><span class="sxs-lookup"><span data-stu-id="88367-113">Click **Sign-up policies** or **Sign-up or sign-in policies** depending on what you configured for sign-up.</span></span>
3. <span data-ttu-id="88367-114">Klikněte na tlačítko vaše zásady (například "B2C_1_SiUp") a ten se otevře.</span><span class="sxs-lookup"><span data-stu-id="88367-114">Click your policy (for example, "B2C_1_SiUp") to open it.</span></span> <span data-ttu-id="88367-115">Klikněte na tlačítko **upravit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="88367-115">Click **Edit** at the top of the blade.</span></span>
4. <span data-ttu-id="88367-116">Klikněte na tlačítko **přizpůsobení uživatelského rozhraní stránky**.</span><span class="sxs-lookup"><span data-stu-id="88367-116">Click **Page UI Customization**.</span></span>
5. <span data-ttu-id="88367-117">Klikněte na tlačítko **stránku pro přihlášení místní účet**.</span><span class="sxs-lookup"><span data-stu-id="88367-117">Click **Local account sign-up page**.</span></span>
6. <span data-ttu-id="88367-118">Klikněte na tlačítko **e-mailovou adresu** v **název** sloupce pod **atributy registrace** části.</span><span class="sxs-lookup"><span data-stu-id="88367-118">Click **Email Address** in the **Name** column under the **Sign-up attributes** section.</span></span>
7. <span data-ttu-id="88367-119">Přepnutí **vyžadovat ověření** možnost k **ne**.</span><span class="sxs-lookup"><span data-stu-id="88367-119">Toggle the **Require verification** option to **No**.</span></span>
8. <span data-ttu-id="88367-120">Klikněte na tlačítko **OK** v dolní části, dokud se nedostanete **upravit zásady** okno.</span><span class="sxs-lookup"><span data-stu-id="88367-120">Click **OK** at the bottom until you reach the **Edit policy** blade.</span></span>
9. <span data-ttu-id="88367-121">Klikněte na tlačítko **Uložit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="88367-121">Click **Save** at the top of the blade.</span></span> <span data-ttu-id="88367-122">Hotovo!</span><span class="sxs-lookup"><span data-stu-id="88367-122">You're done!</span></span>

> [!NOTE]
> <span data-ttu-id="88367-123">Zakázání e-mailu ověření v procesu registrace může vést k zasílání nevyžádané pošty.</span><span class="sxs-lookup"><span data-stu-id="88367-123">Disabling email verification in the sign-up process may lead to spam.</span></span> <span data-ttu-id="88367-124">Pokud zakážete výchozí nastavení, doporučujeme přidání vlastního ověřovacího systému.</span><span class="sxs-lookup"><span data-stu-id="88367-124">If you disable the default one, we recommend adding your own verification system.</span></span>
> 
> 

<span data-ttu-id="88367-125">Snažíme se vždy otevřený a názory a návrhy!</span><span class="sxs-lookup"><span data-stu-id="88367-125">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="88367-126">Pokud máte jakékoli problémy s tímto tématem nebo doporučení pro zlepšení tohoto obsahu, by nám chcete sdělit svůj názor v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="88367-126">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="88367-127">Pro žádosti o funkce, přidejte je do [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="88367-127">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>