---
title: "Azure Active Directory B2C: Vlastní atributy | Microsoft Docs"
description: "Jak používat vlastní atributy ke shromažďování informací o uživatelích v Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 356aaeff3a78fc7b682d621e8e0de9312582b2fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a><span data-ttu-id="f1408-103">Azure Active Directory B2C: Použijte vlastní atributy ke shromažďování informací o uživatelích</span><span class="sxs-lookup"><span data-stu-id="f1408-103">Azure Active Directory B2C: Use custom attributes to collect information about your consumers</span></span>
<span data-ttu-id="f1408-104">Adresáře Azure Active Directory (Azure AD) B2C se dodává s integrovanou sadu informace (atributy): křestní jméno, příjmení, Město, PSČ a další atributy.</span><span class="sxs-lookup"><span data-stu-id="f1408-104">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of information (attributes): Given Name, Surname, City, Postal Code, and other attributes.</span></span> <span data-ttu-id="f1408-105">Každá aplikace určených však má jedinečné požadavky na atributy, které ke shromáždění od příjemce.</span><span class="sxs-lookup"><span data-stu-id="f1408-105">However, every consumer-facing application has unique requirements on what attributes to gather from consumers.</span></span> <span data-ttu-id="f1408-106">S Azure AD B2C můžete rozšířit sadu atributů, které jsou uložené na každý uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="f1408-106">With Azure AD B2C, you can extend the set of attributes stored on each consumer account.</span></span> <span data-ttu-id="f1408-107">Můžete vytvořit vlastní atributy na [portál Azure](https://portal.azure.com/) a použít ho v registraci zásady, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f1408-107">You can create custom attributes on the [Azure portal](https://portal.azure.com/) and use it in your sign-up policies, as shown below.</span></span> <span data-ttu-id="f1408-108">Můžete také číst a zapsat pomocí těchto atributů [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="f1408-108">You can also read and write these attributes by using the [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f1408-109">Vlastní atributy použití [Azure AD Graph API rozšíření schématu služby Directory](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1408-109">Custom attributes use [Azure AD Graph API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span></span>
> 
> 

## <a name="create-a-custom-attribute"></a><span data-ttu-id="f1408-110">Vytvořit vlastní atribut</span><span class="sxs-lookup"><span data-stu-id="f1408-110">Create a custom attribute</span></span>
1. <span data-ttu-id="f1408-111">[Postupujte podle těchto kroků přejděte do okna s funkcemi B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="f1408-111">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="f1408-112">Klikněte na tlačítko **uživatelské atributy**.</span><span class="sxs-lookup"><span data-stu-id="f1408-112">Click **User attributes**.</span></span>
3. <span data-ttu-id="f1408-113">Klikněte na **Přidat** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="f1408-113">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="f1408-114">Zadejte **název** pro vlastní atribut (například "ShoeSize") a volitelně **popis**.</span><span class="sxs-lookup"><span data-stu-id="f1408-114">Provide a **Name** for the custom attribute (for example, "ShoeSize") and optionally, a **Description**.</span></span> <span data-ttu-id="f1408-115">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f1408-115">Click **Create**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f1408-116">Pouze "řetězec" **datový typ** aktuálně nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f1408-116">Only the "String" **Data Type** is currently available.</span></span>
   > 
   > 

<span data-ttu-id="f1408-117">Vlastní atribut je nyní k dispozici v seznamu **uživatelské atributy**a pro použití ve vaší registrační zásady.</span><span class="sxs-lookup"><span data-stu-id="f1408-117">The custom attribute is now available in the list of **User attributes**, and for use in your sign-up policies.</span></span>

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a><span data-ttu-id="f1408-118">Použít vlastní atribut ve svojí registrační zásadě</span><span class="sxs-lookup"><span data-stu-id="f1408-118">Use a custom attribute in your sign-up policy</span></span>
1. <span data-ttu-id="f1408-119">[Postupujte podle těchto kroků přejděte do okna s funkcemi B2C na portálu Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="f1408-119">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="f1408-120">Klikněte na **Zásady registrace**.</span><span class="sxs-lookup"><span data-stu-id="f1408-120">Click **Sign-up policies**.</span></span>
3. <span data-ttu-id="f1408-121">Klikněte na tlačítko svojí registrační zásadě (například "B2C_1_SiUp") a ten se otevře.</span><span class="sxs-lookup"><span data-stu-id="f1408-121">Click your sign-up policy (for example, "B2C_1_SiUp") to open it.</span></span> <span data-ttu-id="f1408-122">Klikněte na tlačítko **upravit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="f1408-122">Click **Edit** at the top of the blade.</span></span>
4. <span data-ttu-id="f1408-123">Klikněte na tlačítko **atributy registrace** a vyberte vlastní atribut (například "ShoeSize").</span><span class="sxs-lookup"><span data-stu-id="f1408-123">Click **Sign-up attributes** and select the custom attribute (for example, "ShoeSize").</span></span> <span data-ttu-id="f1408-124">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1408-124">Click **OK**.</span></span>
5. <span data-ttu-id="f1408-125">Klikněte na tlačítko **deklarace identity aplikace** a vyberte vlastní atribut.</span><span class="sxs-lookup"><span data-stu-id="f1408-125">Click **Application claims** and select the custom attribute.</span></span> <span data-ttu-id="f1408-126">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1408-126">Click **OK**.</span></span>
6. <span data-ttu-id="f1408-127">Klikněte na tlačítko **Uložit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="f1408-127">Click **Save** at the top of the blade.</span></span>

<span data-ttu-id="f1408-128">Funkci "Spustit nyní" v zásadách slouží k ověření prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1408-128">You can use the "Run now" feature on the policy to verify the consumer experience.</span></span> <span data-ttu-id="f1408-129">Teď by měla v seznamu atributů shromážděná během registrace uživatelů najdete v části "ShoeSize" a najdete v části v tokenu odeslána zpět do vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1408-129">You should now see "ShoeSize" in the list of attributes collected during consumer sign-up, and see it in the token sent back to your application.</span></span>

## <a name="notes"></a><span data-ttu-id="f1408-130">Poznámky</span><span class="sxs-lookup"><span data-stu-id="f1408-130">Notes</span></span>
* <span data-ttu-id="f1408-131">Společně s registraci zásady vlastní atributy lze také v zásadách registrace nebo přihlášení a zásady pro úpravy profilu.</span><span class="sxs-lookup"><span data-stu-id="f1408-131">Along with sign-up policies, custom attributes can also be used in sign-up or sign-in policies and profile editing policies.</span></span>
* <span data-ttu-id="f1408-132">Není o známé omezení vlastních atributů.</span><span class="sxs-lookup"><span data-stu-id="f1408-132">There is a known limitation of custom attributes.</span></span> <span data-ttu-id="f1408-133">Je jen vytvořen při prvním se používá v žádné zásady, a ne v případě, že ho přidáte do seznamu **uživatelské atributy**.</span><span class="sxs-lookup"><span data-stu-id="f1408-133">It is only created the first time it is used in any policy, and not when you add it to the list of **User attributes**.</span></span>

