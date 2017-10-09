---
title: "vaše přihlášení stránku hello Azure Active Directory aaaCustomize | Microsoft Docs"
description: "Zjistěte, jak tooadd na stránce firemního brandingu toohello Azure přihlášení"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 151521e3b9cbc6a438a589735058fbff78443cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a><span data-ttu-id="b2942-103">Přidání firemního brandingu tooyour přihlašovací stránky v hello Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b2942-103">Add company branding tooyour sign-in page in hello Azure Active Directory</span></span>
<span data-ttu-id="b2942-104">tooavoid nedorozuměním mnoho společností má tooapply konzistentní vzhled a chování ve všech hello webů a služeb, které spravují.</span><span class="sxs-lookup"><span data-stu-id="b2942-104">tooavoid confusion, many companies want tooapply a consistent look and feel across all hello websites and services they manage.</span></span> <span data-ttu-id="b2942-105">Tato funkce poskytuje Azure Active Directory tak, že umožní toocustomize hello vzhled hello přihlašovací stránka s svoje firemní logo a vlastní barevná schémata.</span><span class="sxs-lookup"><span data-stu-id="b2942-105">Azure Active Directory provides this capability by allowing you toocustomize hello appearance of hello sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="b2942-106">přihlašovací stránku Hello je hello stránka, která se zobrazí při přihlášení tooOffice 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="b2942-106">hello sign-in page is hello page that appears when you sign in tooOffice 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="b2942-107">Budete používat tuto stránku tooenter přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b2942-107">You interact with this page tooenter your credentials.</span></span>

<span data-ttu-id="b2942-108">Pokud chcete tooshow vaší společnosti značku, barvy a další přizpůsobitelné prvky na této stránce, najdete v části hello následující obrázky toounderstand hello rozdíl mezi oběma prostředími hello.</span><span class="sxs-lookup"><span data-stu-id="b2942-108">If you want tooshow your company brand, colors and other customizable elements on this page, see hello following images toounderstand hello difference between hello two experiences.</span></span>

<span data-ttu-id="b2942-109">Následující snímek obrazovky ukazuje příklad stránky přihlašovací hello Office 365 na stolním počítači a Hello **před** přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="b2942-109">hello following screenshot shows and example for hello Office 365 sign-in page on a desktop computer **before** a customization:</span></span>

![Přihlašovací stránka Office 365 před přizpůsobením](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

<span data-ttu-id="b2942-111">Následující snímek obrazovky ukazuje příklad stránky přihlašovací hello Office 365 na stolním počítači a Hello **po** přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="b2942-111">hello following screenshot shows and example for hello Office 365 sign-in page on a desktop computer **after** a customization:</span></span>

![Přihlašovací stránka Office 365 po přizpůsobení](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-hello-sign-in-page"></a><span data-ttu-id="b2942-113">Přizpůsobení přihlašovací stránku hello</span><span class="sxs-lookup"><span data-stu-id="b2942-113">Customizing hello sign-in page</span></span>
<span data-ttu-id="b2942-114">Pokud potřebujete přístup založené na prohlížeči tooyour cloudových aplikací a služeb, které vaše organizace předplatila, obvykle použijete přihlašovací stránku hello.</span><span class="sxs-lookup"><span data-stu-id="b2942-114">Typically, if you need browser-based access tooyour cloud apps and services that your organization subscribes to, you use hello sign-in page.</span></span>

<span data-ttu-id="b2942-115">Pokud jste použili změny tooyour přihlašovací stránky, může trvat až hodinu tooan tooappear změny hello.</span><span class="sxs-lookup"><span data-stu-id="b2942-115">If you have applied changes tooyour sign-in page, it can take up tooan hour for hello changes tooappear.</span></span>

<span data-ttu-id="b2942-116">Přihlašovací stránka ve vaší firemní úpravě se zobrazí jenom tehdy, když službu navštívíte pomocí adresy URL konkrétního klienta, například https://outlook.com/**contoso**.com nebo https://mail.**contoso**.com.</span><span class="sxs-lookup"><span data-stu-id="b2942-116">A branded sign-in page only appears when you visit a service with a tenant-specific URL such as https://outlook.com/**contoso**.com, or https://mail.**contoso**.com.</span></span>

<span data-ttu-id="b2942-117">Když službu navštívíte pomocí adresy URL, která se neváže ke konkrétnímu klientu (např: https://mail.office365.com), zobrazí se přihlašovací stránka bez firemní úpravy.</span><span class="sxs-lookup"><span data-stu-id="b2942-117">When you visit a service with non-tenant specific URLs (e.g.: https://mail.office365.com), a non-branded sign-in page appears.</span></span> <span data-ttu-id="b2942-118">V tomto případě se branding zobrazí až potom, co zadáte ID uživatele nebo vyberete dlaždici uživatele.</span><span class="sxs-lookup"><span data-stu-id="b2942-118">in this case, your branding appears once you have entered your user ID or you have selected a user tile.</span></span>

> [!NOTE]
> * <span data-ttu-id="b2942-119">Název domény musí zobrazovat jako "Aktivní" v hello **domén** část hello portálu Azure, ve kterém jste branding nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="b2942-119">Your domain name must appear as “Active" in hello **Domains** portion of hello Azure portal in which you have configured branding.</span></span> <span data-ttu-id="b2942-120">Další informace najdete v tématu [přidat vlastní názvy domén](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b2942-120">For more information, see [Add custom domain names](active-directory-domains-add-azure-portal.md).</span></span>
> * <span data-ttu-id="b2942-121">Branding přihlašovací stránky se nepřenáší toohello spotřebitelskou přihlašovací stránku společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2942-121">Sign-in page branding doesn’t carry over toohello consumer sign in page of Microsoft.</span></span> <span data-ttu-id="b2942-122">Pokud se přihlásíte pomocí účtu Microsoft, mohou se zobrazit seznam uživatelských dlaždic vykreslí Azure AD partnerské, ale hello branding vaší organizace se nevztahuje toohello stránky účtu Microsoft přihlásit.</span><span class="sxs-lookup"><span data-stu-id="b2942-122">If you sign in with a Microsoft account, you may see a branded list of user tiles rendered by Azure AD, but hello branding of your organization does not apply toohello Microsoft account sign-in page.</span></span>
>
>

<span data-ttu-id="b2942-123">Na stránku přihlášení hello **zůstat přihlášeni** políčko umožňuje uživatele tooremain, při jejich zavřete a znovu ho otevřete svého prohlížeče přihlášený.</span><span class="sxs-lookup"><span data-stu-id="b2942-123">On your sign-in page, hello **Keep me signed in** checkbox allows a user tooremain signed in when they close and re-open their browser.</span></span>

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

<span data-ttu-id="b2942-125">Na životnost relace to vliv nemá.</span><span class="sxs-lookup"><span data-stu-id="b2942-125">It does not effect session lifetime.</span></span> <span data-ttu-id="b2942-126">Můžete skrýt hello zaškrtávací políčko je na hello Azure Active Directory přihlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="b2942-126">You can hide hello checkbox on hello Azure Active Directory sign-in page.</span></span>
<span data-ttu-id="b2942-127">Jestli se zobrazí zaškrtávací políčko hello závisí na nastavení hello **zůstat přihlášeni zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="b2942-127">Whether hello checkbox is displayed depends on hello setting of **Keep me signed in disabled**.</span></span>

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

<span data-ttu-id="b2942-129">toohide hello zaškrtávací políčko, nakonfigurujte toto nastavení příliš**Ano**.</span><span class="sxs-lookup"><span data-stu-id="b2942-129">toohide hello checkbox, configure this setting too**Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b2942-130">Některé funkce služby SharePoint Online a Office 2010, závisí na uživatele, je možné toocheck toto políčko.</span><span class="sxs-lookup"><span data-stu-id="b2942-130">Some features of SharePoint Online and Office 2010 depend on users being able toocheck this box.</span></span> <span data-ttu-id="b2942-131">Pokud nakonfigurujete toto nastavení toohidden, může se uživatelům zobrazí další a neočekávané výzvy toosign v.</span><span class="sxs-lookup"><span data-stu-id="b2942-131">If you configure this setting toohidden, your users may see additional and unexpected prompts toosign-in.</span></span>
>
>

<span data-ttu-id="b2942-132">**tooadd firemního brandingu tooyour adresář:**</span><span class="sxs-lookup"><span data-stu-id="b2942-132">**tooadd company branding tooyour directory:**</span></span>

1. <span data-ttu-id="b2942-133">Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.</span><span class="sxs-lookup"><span data-stu-id="b2942-133">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="b2942-134">Vyberte **další služby**, zadejte **uživatelů a skupin** v hello textového pole a pak vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b2942-134">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. <span data-ttu-id="b2942-136">Na hello **uživatelů a skupin** vyberte **firemní branding**.</span><span class="sxs-lookup"><span data-stu-id="b2942-136">On hello **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="b2942-137">Na hello **uživatelé a skupiny - firemní branding** okně, vyberte hello **upravit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b2942-137">On hello **Users and groups - Company branding** blade, select hello **Edit** command.</span></span>

    ![Upravit vlastní branding](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. <span data-ttu-id="b2942-139">Upravte prvky hello chcete toocustomize.</span><span class="sxs-lookup"><span data-stu-id="b2942-139">Modify hello elements you want toocustomize.</span></span> <span data-ttu-id="b2942-140">Všechny prvky jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="b2942-140">All elements are optional.</span></span>
6. <span data-ttu-id="b2942-141">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="b2942-141">Click **Save**.</span></span>

<span data-ttu-id="b2942-142">Může to trvat až hodinu tooan pro všechny změny provedené toohello přihlašovací stránka brandingu tooappear.</span><span class="sxs-lookup"><span data-stu-id="b2942-142">It can take up tooan hour for any changes you made toohello sign-in page branding tooappear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2942-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2942-143">Next steps</span></span>
[<span data-ttu-id="b2942-144">Přidání brandingu firmy konkrétní jazyk</span><span class="sxs-lookup"><span data-stu-id="b2942-144">Add language-specific company branding</span></span>](active-directory-branding-localize-azure-portal.md)
