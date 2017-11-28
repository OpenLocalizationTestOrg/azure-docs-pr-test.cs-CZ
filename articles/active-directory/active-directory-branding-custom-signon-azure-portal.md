---
title: "Přizpůsobit přihlašovací stránka ve službě Azure Active Directory | Microsoft Docs"
description: "Informace o postupu přidání firemního brandingu na stránky Azure přihlášení"
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
ms.openlocfilehash: 27590c018ea55e9793246c7a4cab10f934ea502b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a><span data-ttu-id="862bc-103">Přidání firemního brandingu na přihlašovací stránku ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="862bc-103">Add company branding to your sign-in page in the Azure Active Directory</span></span>
<span data-ttu-id="862bc-104">Mnoho společností chce předcházet zmatení uživatele a upřednostňuje jednotný vzhled všech webů a služeb, které spravují.</span><span class="sxs-lookup"><span data-stu-id="862bc-104">To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage.</span></span> <span data-ttu-id="862bc-105">Tato funkce poskytuje Azure Active Directory tím, že se můžete přizpůsobit vzhled stránky přihlášení s svoje firemní logo a vlastní barevná schémata.</span><span class="sxs-lookup"><span data-stu-id="862bc-105">Azure Active Directory provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="862bc-106">Přihlašovací stránka je stránka, která se zobrazí při přihlášení k Office 365 nebo jiné webové aplikace, které používají Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="862bc-106">The sign-in page is the page that appears when you sign in to Office 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="862bc-107">Budete používat tuto stránku k zadání pověření.</span><span class="sxs-lookup"><span data-stu-id="862bc-107">You interact with this page to enter your credentials.</span></span>

<span data-ttu-id="862bc-108">Pokud chcete na této stránce zobrazit značku, barvy a další přizpůsobitelné prvky vaší společnosti, prohlédněte si následující obrázky, abyste pochopili rozdíl mezi oběma prostředími.</span><span class="sxs-lookup"><span data-stu-id="862bc-108">If you want to show your company brand, colors and other customizable elements on this page, see the following images to understand the difference between the two experiences.</span></span>

<span data-ttu-id="862bc-109">Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na stolním počítači **před** přizpůsobením:</span><span class="sxs-lookup"><span data-stu-id="862bc-109">The following screenshot shows and example for the Office 365 sign-in page on a desktop computer **before** a customization:</span></span>

![Přihlašovací stránka Office 365 před přizpůsobením](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

<span data-ttu-id="862bc-111">Následující snímek obrazovky ukazuje příklad přihlašovací stránky Office 365 na stolním počítači **po** přizpůsobení:</span><span class="sxs-lookup"><span data-stu-id="862bc-111">The following screenshot shows and example for the Office 365 sign-in page on a desktop computer **after** a customization:</span></span>

![Přihlašovací stránka Office 365 po přizpůsobení](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a><span data-ttu-id="862bc-113">Přizpůsobení přihlašovací stránky</span><span class="sxs-lookup"><span data-stu-id="862bc-113">Customizing the sign-in page</span></span>
<span data-ttu-id="862bc-114">Pokud potřebujete v prohlížeči otevřít cloudové aplikace a služby, které si vaše organizace předplatila, obvykle použijete přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="862bc-114">Typically, if you need browser-based access to your cloud apps and services that your organization subscribes to, you use the sign-in page.</span></span>

<span data-ttu-id="862bc-115">Pokud jste přihlašovací stránku změnili, může se taková změna projevit až za hodinu.</span><span class="sxs-lookup"><span data-stu-id="862bc-115">If you have applied changes to your sign-in page, it can take up to an hour for the changes to appear.</span></span>

<span data-ttu-id="862bc-116">Přihlašovací stránka ve vaší firemní úpravě se zobrazí jenom tehdy, když službu navštívíte pomocí adresy URL konkrétního klienta, například https://outlook.com/**contoso**.com nebo https://mail.**contoso**.com.</span><span class="sxs-lookup"><span data-stu-id="862bc-116">A branded sign-in page only appears when you visit a service with a tenant-specific URL such as https://outlook.com/**contoso**.com, or https://mail.**contoso**.com.</span></span>

<span data-ttu-id="862bc-117">Když službu navštívíte pomocí adresy URL, která se neváže ke konkrétnímu klientu (např: https://mail.office365.com), zobrazí se přihlašovací stránka bez firemní úpravy.</span><span class="sxs-lookup"><span data-stu-id="862bc-117">When you visit a service with non-tenant specific URLs (e.g.: https://mail.office365.com), a non-branded sign-in page appears.</span></span> <span data-ttu-id="862bc-118">V tomto případě se branding zobrazí až potom, co zadáte ID uživatele nebo vyberete dlaždici uživatele.</span><span class="sxs-lookup"><span data-stu-id="862bc-118">in this case, your branding appears once you have entered your user ID or you have selected a user tile.</span></span>

> [!NOTE]
> * <span data-ttu-id="862bc-119">Název domény musí zobrazovat jako "Aktivní" v **domén** část portálu Azure, ve kterém jste branding nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="862bc-119">Your domain name must appear as “Active" in the **Domains** portion of the Azure portal in which you have configured branding.</span></span> <span data-ttu-id="862bc-120">Další informace najdete v tématu [přidat vlastní názvy domén](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="862bc-120">For more information, see [Add custom domain names](active-directory-domains-add-azure-portal.md).</span></span>
> * <span data-ttu-id="862bc-121">Branding přihlašovací stránky se nepřenáší na spotřebitelskou přihlašovací stránku Microsoftu.</span><span class="sxs-lookup"><span data-stu-id="862bc-121">Sign-in page branding doesn’t carry over to the consumer sign in page of Microsoft.</span></span> <span data-ttu-id="862bc-122">Pokud se přihlásíte pomocí účtu Microsoft, mohou se zobrazit seznam uživatelských dlaždic vykreslí Azure AD partnerské, ale branding vaší organizace nevztahuje na stránku účtu Microsoft přihlásit.</span><span class="sxs-lookup"><span data-stu-id="862bc-122">If you sign in with a Microsoft account, you may see a branded list of user tiles rendered by Azure AD, but the branding of your organization does not apply to the Microsoft account sign-in page.</span></span>
>
>

<span data-ttu-id="862bc-123">Na přihlašovací stránce umožňuje zaškrtávací políčko **Zůstat přihlášeni**, aby příslušný uživatel zůstal přihlášen i po zavření a dalším spuštění prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="862bc-123">On your sign-in page, the **Keep me signed in** checkbox allows a user to remain signed in when they close and re-open their browser.</span></span>

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/01.png)

<span data-ttu-id="862bc-125">Na životnost relace to vliv nemá.</span><span class="sxs-lookup"><span data-stu-id="862bc-125">It does not effect session lifetime.</span></span> <span data-ttu-id="862bc-126">Příslušné zaškrtávací políčko na přihlašovací stránce služby Azure Active Directory lze skrýt.</span><span class="sxs-lookup"><span data-stu-id="862bc-126">You can hide the checkbox on the Azure Active Directory sign-in page.</span></span>
<span data-ttu-id="862bc-127">Jestli se zobrazí zaškrtávací políčko závisí na nastavení **zůstat přihlášeni zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="862bc-127">Whether the checkbox is displayed depends on the setting of **Keep me signed in disabled**.</span></span>

   ![Zůstat přihlášeni](./media/active-directory-branding-custom-signon-azure-portal/02.png)

<span data-ttu-id="862bc-129">Skrýt políčka, konfigurací tohoto nastavení **Ano**.</span><span class="sxs-lookup"><span data-stu-id="862bc-129">To hide the checkbox, configure this setting to **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="862bc-130">Některé funkce služeb SharePoint Online a Office 2010 závisí na tom, zda uživatelé mohou toto políčko zaškrtnout.</span><span class="sxs-lookup"><span data-stu-id="862bc-130">Some features of SharePoint Online and Office 2010 depend on users being able to check this box.</span></span> <span data-ttu-id="862bc-131">Pokud je nastavíte jako skryté, mohou se vašim uživatelům zobrazovat další (neočekávané) výzvy k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="862bc-131">If you configure this setting to hidden, your users may see additional and unexpected prompts to sign-in.</span></span>
>
>

<span data-ttu-id="862bc-132">**Postup přidání firemního brandingu na adresáře:**</span><span class="sxs-lookup"><span data-stu-id="862bc-132">**To add company branding to your directory:**</span></span>

1. <span data-ttu-id="862bc-133">Přihlaste se k [portál Azure](https://portal.azure.com) pomocí účtu, který je globální správce adresáře.</span><span class="sxs-lookup"><span data-stu-id="862bc-133">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="862bc-134">Vyberte **další služby**, zadejte **uživatelů a skupin** v textovém poli a potom vyberte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="862bc-134">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Správa uživatelů otevírání](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. <span data-ttu-id="862bc-136">Na **uživatelů a skupin** vyberte **firemní branding**.</span><span class="sxs-lookup"><span data-stu-id="862bc-136">On the **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="862bc-137">Na **uživatelé a skupiny - firemní branding** okně, vyberte **upravit** příkaz.</span><span class="sxs-lookup"><span data-stu-id="862bc-137">On the **Users and groups - Company branding** blade, select the **Edit** command.</span></span>

    ![Upravit vlastní branding](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. <span data-ttu-id="862bc-139">Upravte prvky, které chcete přizpůsobit.</span><span class="sxs-lookup"><span data-stu-id="862bc-139">Modify the elements you want to customize.</span></span> <span data-ttu-id="862bc-140">Všechny prvky jsou volitelné.</span><span class="sxs-lookup"><span data-stu-id="862bc-140">All elements are optional.</span></span>
6. <span data-ttu-id="862bc-141">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="862bc-141">Click **Save**.</span></span>

<span data-ttu-id="862bc-142">Může trvat až jednu hodinu pro všechny změny, které jste udělali na přihlašovací stránku branding zobrazí.</span><span class="sxs-lookup"><span data-stu-id="862bc-142">It can take up to an hour for any changes you made to the sign-in page branding to appear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="862bc-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="862bc-143">Next steps</span></span>
[<span data-ttu-id="862bc-144">Přidání brandingu firmy konkrétní jazyk</span><span class="sxs-lookup"><span data-stu-id="862bc-144">Add language-specific company branding</span></span>](active-directory-branding-localize-azure-portal.md)
