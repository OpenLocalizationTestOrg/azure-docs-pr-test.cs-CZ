---
title: "Přidání konkrétní jazyk firemního brandingu na přihlašovací stránku ve službě Azure Active Directory | Microsoft Docs"
description: "Informace o přidání určité společnosti jazyk branding obrázky a text na stránku Azure přihlášení"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a><span data-ttu-id="c2bb1-103">Přidání konkrétní jazyk firemního brandingu na přihlašovací stránku ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c2bb1-103">Add language-specific company branding to your sign-in page in the Azure Active Directory</span></span>
<span data-ttu-id="c2bb1-104">Mnoho společností chce předcházet zmatení uživatele a upřednostňuje jednotný vzhled všech webů a služeb, které spravují.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-104">To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage.</span></span> <span data-ttu-id="c2bb1-105">Tato funkce poskytuje Azure Active Directory tím, že se můžete přizpůsobit vzhled stránky přihlášení s svoje firemní logo a vlastní barevná schémata.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-105">Azure Active Directory provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="c2bb1-106">Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-106">The sign-in page is the page that appears when you sign in to Office 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="c2bb1-107">Budete používat tuto stránku k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-107">You interact with this page to enter your credentials.</span></span>

## <a name="customizing-the-sign-in-page-for-another-language"></a><span data-ttu-id="c2bb1-108">Přizpůsobení přihlašovací stránky pro jiný jazyk</span><span class="sxs-lookup"><span data-stu-id="c2bb1-108">Customizing the sign-in page for another language</span></span>
<span data-ttu-id="c2bb1-109">Elementy jazyka můžete přidat vlastní přihlašovací stránku pouze v případě, že jste již vytvořili vlastní stránku přihlášení, jak je popsáno v [přidání firemního brandingu na přihlašovací stránce](active-directory-branding-custom-signon-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c2bb1-109">You can add language-specific elements to your custom sign-in page only if you have already created a custom sign-in page as described in [Add company branding to your sign-in page](active-directory-branding-custom-signon-azure-portal.md).</span></span> <span data-ttu-id="c2bb1-110">Jednu stránku přihlášení na adresář můžete nakonfigurovat výchozí sadu přizpůsobitelných prvků.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-110">You can configure one sign-in page per directory with a default set of customizable elements.</span></span> <span data-ttu-id="c2bb1-111">Po dokončení konfigurace výchozí sadu elementů stránky, můžete nakonfigurovat další verze pro různá národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-111">After you’ve configured the default set of page elements, you can configure more versions for different locales.</span></span> <span data-ttu-id="c2bb1-112">Různé prvky mezi sebou můžete kombinovat.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-112">You can also mix and match various elements.</span></span> <span data-ttu-id="c2bb1-113">Například je může:</span><span class="sxs-lookup"><span data-stu-id="c2bb1-113">For example, you could:</span></span>

* <span data-ttu-id="c2bb1-114">Vytvoří výchozí **přihlašovací stránky image** , platí pro všechny kultury, potom vytvořte specifické verze pro angličtinu a francouzštinu.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-114">Create a default **Sign-in page image** that works for all cultures, then create specific versions for English and French.</span></span> <span data-ttu-id="c2bb1-115">Když svoje prohlížeče nastavíte na jednu z těchto dvou jazyků, bitovou kopii pro specifický jazyk se zobrazí, když u všech ostatních jazyků se zobrazí výchozí obrázek.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-115">When you set your browsers to one of these two languages, the language-specific image appears, while the default illustration appears for all other languages.</span></span>
* <span data-ttu-id="c2bb1-116">Nakonfigurujte různá loga vaší organizace (třeba verze pro japonštinu nebo hebrejštinu).</span><span class="sxs-lookup"><span data-stu-id="c2bb1-116">Configure different logos for your organization (for example, Japanese or Hebrew versions).</span></span>

<span data-ttu-id="c2bb1-117">Doporučujeme, aby byl počet variant jazyk nízké, údržby a lepšího výkonu.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-117">We recommend that you keep the number of language variations low, for maintenance and performance reasons.</span></span>

<span data-ttu-id="c2bb1-118">**Postup přidání firemního brandingu na adresáře:**</span><span class="sxs-lookup"><span data-stu-id="c2bb1-118">**To add company branding to your directory:**</span></span>

1. <span data-ttu-id="c2bb1-119">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-119">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="c2bb1-120">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-120">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. <span data-ttu-id="c2bb1-122">Na **uživatelů a skupin** vyberte **firemní branding**.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-122">On the **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="c2bb1-123">Na **uživatelé a skupiny - firemní branding** okně, vyberte **přidat jazyk** příkaz.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-123">On the **Users and groups - Company branding** blade, select the **Add language** command.</span></span>

    ![Přidání brandingu elementy jazyka](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. <span data-ttu-id="c2bb1-125">Upravte prvky, které chcete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-125">Modify the elements you want to customize.</span></span> <span data-ttu-id="c2bb1-126">Všechny prvky jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-126">All elements are optional.</span></span>
6. <span data-ttu-id="c2bb1-127">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-127">Click **Save**.</span></span>

<span data-ttu-id="c2bb1-128">Může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c2bb1-128">It can take up to an hour for any changes you made to the sign-in page branding to appear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2bb1-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c2bb1-129">Next steps</span></span>
[<span data-ttu-id="c2bb1-130">Přidání firemního brandingu na přihlašovací stránce</span><span class="sxs-lookup"><span data-stu-id="c2bb1-130">Add company branding to your sign-in page</span></span>](active-directory-branding-custom-signon-azure-portal.md)
