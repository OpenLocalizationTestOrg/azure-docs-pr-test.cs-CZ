---
title: "Konfigurace aplikací SaaS pro spolupráci B2B ve službě Azure Active Directory | Microsoft Docs"
description: "Ukázky kódu a prostředí PowerShell pro Azure Active Directory s B2B spolupráce"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: 149a493f7b369415f0a2726dd6a576f0195c13d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a><span data-ttu-id="744df-103">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="744df-103">Configure SaaS apps for B2B collaboration</span></span>

<span data-ttu-id="744df-104">Spolupráce Azure Active Directory (Azure AD) s B2B pracuje s většinu aplikací, které se integrují s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="744df-104">Azure Active Directory (Azure AD) B2B collaboration works with most apps that integrate with Azure AD.</span></span> <span data-ttu-id="744df-105">V této části jsme provede pokyny ke konfiguraci některých oblíbených aplikací SaaS pro použití s Azure AD s B2B.</span><span class="sxs-lookup"><span data-stu-id="744df-105">In this section, we walk through instructions for configuring some popular SaaS apps for use with Azure AD B2B.</span></span>

<span data-ttu-id="744df-106">Předtím, než se podíváte na konkrétní aplikaci pokyny, zde jsou některé zásady:</span><span class="sxs-lookup"><span data-stu-id="744df-106">Before you look at app-specific instructions, here are some rules of thumb:</span></span>

* <span data-ttu-id="744df-107">Pro většinu aplikací musí dojít ručně nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="744df-107">For most of the apps, user setup needs to happen manually.</span></span> <span data-ttu-id="744df-108">To znamená uživatelé musí být vytvořeny ručně v aplikaci také.</span><span class="sxs-lookup"><span data-stu-id="744df-108">That is, users must be created manually in the app as well.</span></span>

* <span data-ttu-id="744df-109">Pro aplikace, které podporují automatické nastavení, jako je Dropbox jsou vytvořeny samostatné pozvánek z aplikací.</span><span class="sxs-lookup"><span data-stu-id="744df-109">For apps that support automatic setup, such as Dropbox, separate invitations are created from the apps.</span></span> <span data-ttu-id="744df-110">Uživatelé musí být jisti tak, aby přijímal každý pozvánku.</span><span class="sxs-lookup"><span data-stu-id="744df-110">Users must be sure to accept each invitation.</span></span>

* <span data-ttu-id="744df-111">Atributy uživatele na zmírnit problémy s disk profilu pozměnění uživatele (UPD) v uživatele typu Host, vždy nastavte **uživatelský identifikátor** k **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="744df-111">In the user attributes, to mitigate any issues with mangled user profile disk (UPD) in guest users, always set **User Identifier** to **user.mail**.</span></span>


## <a name="dropbox-business"></a><span data-ttu-id="744df-112">Obchodní dropbox</span><span class="sxs-lookup"><span data-stu-id="744df-112">Dropbox Business</span></span>

<span data-ttu-id="744df-113">Pokud chcete povolit uživatelům přihlášení pomocí účtu organizace, je nutné ručně nakonfigurovat Dropbox obchodní používat Azure AD jako zprostředkovatele identity Security (Assertion Markup Language SAML).</span><span class="sxs-lookup"><span data-stu-id="744df-113">To enable users to sign in using their organization account, you must manually configure Dropbox Business to use Azure AD as a Security Assertion Markup Language (SAML) identity provider.</span></span> <span data-ttu-id="744df-114">Pokud obchodní Dropbox nebyl nakonfigurován k tomu, nemůže výzvu nebo jinak umožňují uživatelům přihlásit se pomocí služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="744df-114">If Dropbox Business has not been configured to do so, it cannot prompt or otherwise allow users to sign in using Azure AD.</span></span>

1. <span data-ttu-id="744df-115">Chcete-li přidat Dropbox obchodní aplikace do služby Azure AD, vyberte **podnikové aplikace, které** v levém podokně a pak klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="744df-115">To add the Dropbox Business app into Azure AD, select **Enterprise applications** in the left pane, and then click **Add**.</span></span>

  ![Tlačítko "Přidat" na stránce podnikových aplikací](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. <span data-ttu-id="744df-117">V **přidat aplikaci** okno, zadejte **dropbox** v vyhledávacího pole a potom vyberte **Dropbox pro firmy** v seznamu výsledků.</span><span class="sxs-lookup"><span data-stu-id="744df-117">In the **Add an application** window, enter **dropbox** in the search box, and then select **Dropbox for Business** in the results list.</span></span>

  ![Vyhledejte "dropbox" na Přidat stránky aplikace](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. <span data-ttu-id="744df-119">Na **jednotného přihlašování** vyberte **jednotného přihlašování** v levém podokně a potom zadejte **user.mail** v **uživatelský identifikátor** pole.</span><span class="sxs-lookup"><span data-stu-id="744df-119">On the **Single sign-on** page, select **Single sign-on** in the left pane, and then enter **user.mail** in the **User Identifier** box.</span></span> <span data-ttu-id="744df-120">(Je nastavený jako UPN ve výchozím nastavení.)</span><span class="sxs-lookup"><span data-stu-id="744df-120">(It's set as UPN by default.)</span></span>

  ![Konfigurace jednotného přihlašování pro aplikace](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. <span data-ttu-id="744df-122">Chcete-li stáhnout certifikát, který chcete použít pro konfiguraci Dropbox, vyberte **konfigurace DropBox**a potom vyberte **SAML jeden znak na adresu URL služby** v seznamu.</span><span class="sxs-lookup"><span data-stu-id="744df-122">To download the certificate to use for Dropbox configuration, select **Configure DropBox**, and then select **SAML Single Sign On Service URL** in the list.</span></span>

  ![Stáhnout certifikát pro Dropbox konfigurace](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. <span data-ttu-id="744df-124">Přihlaste se k Dropboxu s adresou URL přihlašování z **jednotného přihlašování** stránky.</span><span class="sxs-lookup"><span data-stu-id="744df-124">Sign in to Dropbox with the sign-on URL from the **Single sign-on** page.</span></span>

  ![Na přihlašovací stránce Dropbox](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. <span data-ttu-id="744df-126">V nabídce vyberte **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="744df-126">On the menu, select **Admin Console**.</span></span>

  !["Konzoly pro správu" odkaz v nabídce Dropbox](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. <span data-ttu-id="744df-128">V **ověřování** dialogové okno, vyberte **Další**, odešlete certifikát a potom na **přihlašovací adresa URL** zadejte SAML jeden přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="744df-128">In the **Authentication** dialog box, select **More**, upload the certificate and then, in the **Sign in URL** box, enter the SAML single sign-on URL.</span></span>

  ![Odkaz "Informace" v dialogovém okně sbalené ověřování](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  !["Podepsat v adrese URL" v dialogovém okně Rozšířená ověřování](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. <span data-ttu-id="744df-131">Chcete-li konfigurovat nastavení automatické uživatele na portálu Azure, vyberte **zřizování** v levém podokně vyberte **automatické** v **režimu zřizování** a pak vyberte **Autorizovat**.</span><span class="sxs-lookup"><span data-stu-id="744df-131">To configure automatic user setup in the Azure portal, select **Provisioning** in the left pane, select **Automatic** in the **Provisioning Mode** box, and then select **Authorize**.</span></span>

  ![Konfiguraci zřizování automatické uživatelů na portálu Azure](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

<span data-ttu-id="744df-133">Po hosta nebo člen uživatelů nastavili v aplikaci Dropbox, obdrží samostatné pozvánku z Dropbox.</span><span class="sxs-lookup"><span data-stu-id="744df-133">After guest or member users have been set up in the Dropbox app, they receive a separate invitation from Dropbox.</span></span> <span data-ttu-id="744df-134">Pokud chcete používat Dropbox jednotné přihlašování, musí pozvaným uživatelům přijmout pozvání kliknutím na odkaz v ní.</span><span class="sxs-lookup"><span data-stu-id="744df-134">To use Dropbox single sign-on, invitees must accept the invitation by clicking a link in it.</span></span>

## <a name="box"></a><span data-ttu-id="744df-135">Box</span><span class="sxs-lookup"><span data-stu-id="744df-135">Box</span></span>
<span data-ttu-id="744df-136">Můžete povolit uživatelům ověřovat uživatele typu Host pole ke svému účtu Azure AD pomocí federace, která je založená na protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="744df-136">You can enable users to authenticate Box guest users with their Azure AD account by using federation that's based on the SAML protocol.</span></span> <span data-ttu-id="744df-137">V tomto postupu nahrajte Box.com metadat.</span><span class="sxs-lookup"><span data-stu-id="744df-137">In this procedure, you upload metadata to Box.com.</span></span>

1. <span data-ttu-id="744df-138">Přidáte aplikaci Box z podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="744df-138">Add the Box app from the enterprise apps.</span></span>

2. <span data-ttu-id="744df-139">Konfigurovat jednotné přihlašování v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="744df-139">Configure single sign-on in the following order:</span></span>

  ![Konfigurace pole jednotné přihlašování](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 <span data-ttu-id="744df-141">a.</span><span class="sxs-lookup"><span data-stu-id="744df-141">a.</span></span> <span data-ttu-id="744df-142">V **přihlásit na adrese URL** pole, zkontrolujte, že adresa URL přihlašování je správně nastavená pro pole na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="744df-142">In the **Sign on URL** box, ensure that the sign-on URL is set appropriately for Box in the Azure portal.</span></span> <span data-ttu-id="744df-143">Tato adresa URL je adresa URL vaší Box.com klienta.</span><span class="sxs-lookup"><span data-stu-id="744df-143">This URL is the URL of your Box.com tenant.</span></span> <span data-ttu-id="744df-144">Měl by splňovat zásady vytváření názvů *https://.box.com*.</span><span class="sxs-lookup"><span data-stu-id="744df-144">It should follow the naming convention *https://.box.com*.</span></span>  
 <span data-ttu-id="744df-145">**Identifikátor** nelze použít u této aplikace, ale stále zobrazuje jako povinné pole.</span><span class="sxs-lookup"><span data-stu-id="744df-145">The **Identifier** does not apply to this app, but it still appears as a mandatory field.</span></span>

 <span data-ttu-id="744df-146">b.</span><span class="sxs-lookup"><span data-stu-id="744df-146">b.</span></span> <span data-ttu-id="744df-147">V **uživatelský identifikátor** zadejte **user.mail** (pro jednotné přihlašování pro účet hosta).</span><span class="sxs-lookup"><span data-stu-id="744df-147">In the **User identifier** box, enter **user.mail** (for SSO for guest accounts).</span></span>

 <span data-ttu-id="744df-148">c.</span><span class="sxs-lookup"><span data-stu-id="744df-148">c.</span></span> <span data-ttu-id="744df-149">V části **SAML podpisový certifikát**, klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="744df-149">Under **SAML Signing Certificate**, click **Create new certificate**.</span></span>

 <span data-ttu-id="744df-150">d.</span><span class="sxs-lookup"><span data-stu-id="744df-150">d.</span></span> <span data-ttu-id="744df-151">Chcete-li zahájit konfiguraci vašeho klienta Box.com používat Azure AD jako zprostředkovatele identity, stažení souboru metadat a ukládat ho do místního disku.</span><span class="sxs-lookup"><span data-stu-id="744df-151">To begin configuring your Box.com tenant to use Azure AD as an identity provider, download the metadata file and then save it to your local drive.</span></span>

 <span data-ttu-id="744df-152">e.</span><span class="sxs-lookup"><span data-stu-id="744df-152">e.</span></span> <span data-ttu-id="744df-153">Soubor metadat do pole dál podporovat týmu, který nakonfiguruje jednotné přihlašování pro vás.</span><span class="sxs-lookup"><span data-stu-id="744df-153">Forward the metadata file to the Box support team, which configures single sign-on for you.</span></span>

3. <span data-ttu-id="744df-154">Nastavení automatického uživatele Azure AD, v levém podokně, vyberte **zřizování**a potom vyberte **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="744df-154">For Azure AD automatic user setup, in the left pane, select **Provisioning**, and then select **Authorize**.</span></span>

  ![Autorizace Azure AD pro připojení k poli](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

<span data-ttu-id="744df-156">Jako budou pozvaní uživatelé Dropbox musí pozvaným uživatelům pole uplatnit jejich pozvánku z pole aplikace.</span><span class="sxs-lookup"><span data-stu-id="744df-156">Like Dropbox invitees, Box invitees must redeem their invitation from the Box app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="744df-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="744df-157">Next steps</span></span>

<span data-ttu-id="744df-158">Na spolupráci Azure AD B2B najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="744df-158">See the following articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="744df-159">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="744df-159">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="744df-160">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="744df-160">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="744df-161">Přidání uživatele spolupráce B2B k roli</span><span class="sxs-lookup"><span data-stu-id="744df-161">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="744df-162">Delegovat pozvánek spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="744df-162">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="744df-163">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="744df-163">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="744df-164">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="744df-164">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="744df-165">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="744df-165">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="744df-166">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="744df-166">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="744df-167">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="744df-167">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="744df-168">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="744df-168">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
