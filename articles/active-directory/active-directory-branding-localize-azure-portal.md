---
title: "aaaAdd konkrétní jazyk firemního brandingu tooyour přihlašovací stránky v hello Azure Active Directory | Microsoft Docs"
description: "Zjistěte, jak tooadd konkrétní jazyk společnosti brandingu obrázky a text tooan Azure přihlášení stránky"
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
ms.openlocfilehash: 1e33c31abc242e8455290beb1f03760be7b9ac42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-language-specific-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a><span data-ttu-id="e4898-103">Přidání konkrétní jazyk firemního brandingu tooyour přihlašovací stránky v hello Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e4898-103">Add language-specific company branding tooyour sign-in page in hello Azure Active Directory</span></span>
<span data-ttu-id="e4898-104">tooavoid nedorozuměním mnoho společností má tooapply konzistentní vzhled a chování ve všech hello webů a služeb, které spravují.</span><span class="sxs-lookup"><span data-stu-id="e4898-104">tooavoid confusion, many companies want tooapply a consistent look and feel across all hello websites and services they manage.</span></span> <span data-ttu-id="e4898-105">Tato funkce poskytuje Azure Active Directory tak, že umožní toocustomize hello vzhled hello přihlašovací stránka s svoje firemní logo a vlastní barevná schémata.</span><span class="sxs-lookup"><span data-stu-id="e4898-105">Azure Active Directory provides this capability by allowing you toocustomize hello appearance of hello sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="e4898-106">přihlašovací stránku Hello je hello stránka, která se zobrazí při přihlášení tooOffice 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="e4898-106">hello sign-in page is hello page that appears when you sign in tooOffice 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="e4898-107">Budete používat tuto stránku tooenter přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="e4898-107">You interact with this page tooenter your credentials.</span></span>

## <a name="customizing-hello-sign-in-page-for-another-language"></a><span data-ttu-id="e4898-108">Přizpůsobení hello přihlašovací stránky pro jiný jazyk</span><span class="sxs-lookup"><span data-stu-id="e4898-108">Customizing hello sign-in page for another language</span></span>
<span data-ttu-id="e4898-109">Elementy jazyka tooyour vlastní přihlašovací stránky můžete přidat pouze v případě, že jste již vytvořili vlastní stránku přihlášení, jak je popsáno v [přidání firemního brandingu na přihlašovací stránce tooyour](active-directory-branding-custom-signon-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e4898-109">You can add language-specific elements tooyour custom sign-in page only if you have already created a custom sign-in page as described in [Add company branding tooyour sign-in page](active-directory-branding-custom-signon-azure-portal.md).</span></span> <span data-ttu-id="e4898-110">Jednu stránku přihlášení na adresář můžete nakonfigurovat výchozí sadu přizpůsobitelných prvků.</span><span class="sxs-lookup"><span data-stu-id="e4898-110">You can configure one sign-in page per directory with a default set of customizable elements.</span></span> <span data-ttu-id="e4898-111">Po dokončení konfigurace hello výchozí sadu elementů stránky, můžete nakonfigurovat další verze pro různá národní prostředí.</span><span class="sxs-lookup"><span data-stu-id="e4898-111">After you’ve configured hello default set of page elements, you can configure more versions for different locales.</span></span> <span data-ttu-id="e4898-112">Různé prvky mezi sebou můžete kombinovat.</span><span class="sxs-lookup"><span data-stu-id="e4898-112">You can also mix and match various elements.</span></span> <span data-ttu-id="e4898-113">Například je může:</span><span class="sxs-lookup"><span data-stu-id="e4898-113">For example, you could:</span></span>

* <span data-ttu-id="e4898-114">Vytvoří výchozí **přihlašovací stránky image** , platí pro všechny kultury, potom vytvořte specifické verze pro angličtinu a francouzštinu.</span><span class="sxs-lookup"><span data-stu-id="e4898-114">Create a default **Sign-in page image** that works for all cultures, then create specific versions for English and French.</span></span> <span data-ttu-id="e4898-115">Při nastavení vašeho prohlížeče tooone z těchto dvou jazyků, hello konkrétní jazyk image se zobrazí, když u všech ostatních jazyků se zobrazí výchozí obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="e4898-115">When you set your browsers tooone of these two languages, hello language-specific image appears, while hello default illustration appears for all other languages.</span></span>
* <span data-ttu-id="e4898-116">Nakonfigurujte různá loga vaší organizace (třeba verze pro japonštinu nebo hebrejštinu).</span><span class="sxs-lookup"><span data-stu-id="e4898-116">Configure different logos for your organization (for example, Japanese or Hebrew versions).</span></span>

<span data-ttu-id="e4898-117">Doporučujeme, abyste hello počet variant jazyk nízké, údržby a lepšího výkonu.</span><span class="sxs-lookup"><span data-stu-id="e4898-117">We recommend that you keep hello number of language variations low, for maintenance and performance reasons.</span></span>

<span data-ttu-id="e4898-118">**tooadd firemního brandingu tooyour adresář:**</span><span class="sxs-lookup"><span data-stu-id="e4898-118">**tooadd company branding tooyour directory:**</span></span>

1. <span data-ttu-id="e4898-119">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="e4898-119">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="e4898-120">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e4898-120">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. <span data-ttu-id="e4898-122">Na hello **uživatelů a skupin** vyberte **firemní branding**.</span><span class="sxs-lookup"><span data-stu-id="e4898-122">On hello **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="e4898-123">Na hello **uživatelé a skupiny - firemní branding** okně, vyberte hello **přidat jazyk** příkaz.</span><span class="sxs-lookup"><span data-stu-id="e4898-123">On hello **Users and groups - Company branding** blade, select hello **Add language** command.</span></span>

    ![Přidání brandingu elementy jazyka](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. <span data-ttu-id="e4898-125">Upravte prvky hello chcete toocustomize.</span><span class="sxs-lookup"><span data-stu-id="e4898-125">Modify hello elements you want toocustomize.</span></span> <span data-ttu-id="e4898-126">Všechny prvky jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="e4898-126">All elements are optional.</span></span>
6. <span data-ttu-id="e4898-127">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="e4898-127">Click **Save**.</span></span>

<span data-ttu-id="e4898-128">Může to trvat až hodinu tooan pro všechny změny provedené toohello přihlašovací stránka brandingu tooappear.</span><span class="sxs-lookup"><span data-stu-id="e4898-128">It can take up tooan hour for any changes you made toohello sign-in page branding tooappear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4898-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e4898-129">Next steps</span></span>
[<span data-ttu-id="e4898-130">Přidání firemního brandingu na přihlašovací stránce tooyour</span><span class="sxs-lookup"><span data-stu-id="e4898-130">Add company branding tooyour sign-in page</span></span>](active-directory-branding-custom-signon-azure-portal.md)
