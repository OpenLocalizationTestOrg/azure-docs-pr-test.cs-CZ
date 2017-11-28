---
title: "Azure Active Directory B2C: Vlastní atributy | Microsoft Docs"
description: "Jak toouse vlastních atributů v Azure Active Directory B2C toocollect informací o uživatelích"
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
ms.openlocfilehash: fb1bff77ad54c246c6d2f69f39c03eafb76fe6bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-toocollect-information-about-your-consumers"></a><span data-ttu-id="3900c-103">Azure Active Directory B2C: Použijte vlastní atributy toocollect informací o uživatelích</span><span class="sxs-lookup"><span data-stu-id="3900c-103">Azure Active Directory B2C: Use custom attributes toocollect information about your consumers</span></span>
<span data-ttu-id="3900c-104">Adresáře Azure Active Directory (Azure AD) B2C se dodává s integrovanou sadu informace (atributy): křestní jméno, příjmení, Město, PSČ a další atributy.</span><span class="sxs-lookup"><span data-stu-id="3900c-104">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of information (attributes): Given Name, Surname, City, Postal Code, and other attributes.</span></span> <span data-ttu-id="3900c-105">Každá aplikace určených však má jedinečné požadavky na jaké atributy toogather z příjemci.</span><span class="sxs-lookup"><span data-stu-id="3900c-105">However, every consumer-facing application has unique requirements on what attributes toogather from consumers.</span></span> <span data-ttu-id="3900c-106">S Azure AD B2C můžete rozšířit hello sadu atributů, které jsou uložené na každý uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3900c-106">With Azure AD B2C, you can extend hello set of attributes stored on each consumer account.</span></span> <span data-ttu-id="3900c-107">Můžete vytvořit vlastní atributy pro hello [portál Azure](https://portal.azure.com/) a použít ho v registraci zásady, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="3900c-107">You can create custom attributes on hello [Azure portal](https://portal.azure.com/) and use it in your sign-up policies, as shown below.</span></span> <span data-ttu-id="3900c-108">Můžete také číst a zapsat tyto atributy pomocí hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="3900c-108">You can also read and write these attributes by using hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3900c-109">Vlastní atributy použití [Azure AD Graph API rozšíření schématu služby Directory](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span><span class="sxs-lookup"><span data-stu-id="3900c-109">Custom attributes use [Azure AD Graph API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span></span>
> 
> 

## <a name="create-a-custom-attribute"></a><span data-ttu-id="3900c-110">Vytvořit vlastní atribut</span><span class="sxs-lookup"><span data-stu-id="3900c-110">Create a custom attribute</span></span>
1. <span data-ttu-id="3900c-111">[Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="3900c-111">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="3900c-112">Klikněte na tlačítko **uživatelské atributy**.</span><span class="sxs-lookup"><span data-stu-id="3900c-112">Click **User attributes**.</span></span>
3. <span data-ttu-id="3900c-113">Klikněte na tlačítko **+ přidat** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="3900c-113">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="3900c-114">Zadejte **název** pro vlastní atribut hello (například "ShoeSize") a volitelně **popis**.</span><span class="sxs-lookup"><span data-stu-id="3900c-114">Provide a **Name** for hello custom attribute (for example, "ShoeSize") and optionally, a **Description**.</span></span> <span data-ttu-id="3900c-115">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3900c-115">Click **Create**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="3900c-116">Pouze hello "String" **datový typ** aktuálně nejsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3900c-116">Only hello "String" **Data Type** is currently available.</span></span>
   > 
   > 

<span data-ttu-id="3900c-117">vlastní atribut Hello je nyní k dispozici v seznamu hello **uživatelské atributy**a pro použití ve vaší registrační zásady.</span><span class="sxs-lookup"><span data-stu-id="3900c-117">hello custom attribute is now available in hello list of **User attributes**, and for use in your sign-up policies.</span></span>

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a><span data-ttu-id="3900c-118">Použít vlastní atribut ve svojí registrační zásadě</span><span class="sxs-lookup"><span data-stu-id="3900c-118">Use a custom attribute in your sign-up policy</span></span>
1. <span data-ttu-id="3900c-119">[Postupujte podle těchto kroků toonavigate toohello B2C okno s funkcemi na hello portál Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="3900c-119">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="3900c-120">Klikněte na **Zásady registrace**.</span><span class="sxs-lookup"><span data-stu-id="3900c-120">Click **Sign-up policies**.</span></span>
3. <span data-ttu-id="3900c-121">Klikněte na vaší registrační zásadě (například "B2C_1_SiUp") tooopen ho.</span><span class="sxs-lookup"><span data-stu-id="3900c-121">Click your sign-up policy (for example, "B2C_1_SiUp") tooopen it.</span></span> <span data-ttu-id="3900c-122">Klikněte na tlačítko **upravit** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="3900c-122">Click **Edit** at hello top of hello blade.</span></span>
4. <span data-ttu-id="3900c-123">Klikněte na tlačítko **atributy registrace** a vyberte hello vlastních atributů (například "ShoeSize").</span><span class="sxs-lookup"><span data-stu-id="3900c-123">Click **Sign-up attributes** and select hello custom attribute (for example, "ShoeSize").</span></span> <span data-ttu-id="3900c-124">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3900c-124">Click **OK**.</span></span>
5. <span data-ttu-id="3900c-125">Klikněte na tlačítko **deklarace identity aplikace** a vyberte hello vlastních atributů.</span><span class="sxs-lookup"><span data-stu-id="3900c-125">Click **Application claims** and select hello custom attribute.</span></span> <span data-ttu-id="3900c-126">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3900c-126">Click **OK**.</span></span>
6. <span data-ttu-id="3900c-127">Klikněte na tlačítko **Uložit** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="3900c-127">Click **Save** at hello top of hello blade.</span></span>

<span data-ttu-id="3900c-128">Můžete vytvořit funkci "Spustit nyní" hello hello zásad tooverify hello prostředí pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="3900c-128">You can use hello "Run now" feature on hello policy tooverify hello consumer experience.</span></span> <span data-ttu-id="3900c-129">Teď by měl vidět "ShoeSize" hello seznam atributů shromážděná během registrace příjemce a najdete v části v tokenu odeslaných zpět tooyour aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="3900c-129">You should now see "ShoeSize" in hello list of attributes collected during consumer sign-up, and see it in hello token sent back tooyour application.</span></span>

## <a name="notes"></a><span data-ttu-id="3900c-130">Poznámky</span><span class="sxs-lookup"><span data-stu-id="3900c-130">Notes</span></span>
* <span data-ttu-id="3900c-131">Společně s registraci zásady vlastní atributy lze také v zásadách registrace nebo přihlášení a zásady pro úpravy profilu.</span><span class="sxs-lookup"><span data-stu-id="3900c-131">Along with sign-up policies, custom attributes can also be used in sign-up or sign-in policies and profile editing policies.</span></span>
* <span data-ttu-id="3900c-132">Není o známé omezení vlastních atributů.</span><span class="sxs-lookup"><span data-stu-id="3900c-132">There is a known limitation of custom attributes.</span></span> <span data-ttu-id="3900c-133">Je pouze vytvořené hello poprvé se používá v žádné zásady, a ne v případě, že ho přidáte toohello seznam **uživatelské atributy**.</span><span class="sxs-lookup"><span data-stu-id="3900c-133">It is only created hello first time it is used in any policy, and not when you add it toohello list of **User attributes**.</span></span>

