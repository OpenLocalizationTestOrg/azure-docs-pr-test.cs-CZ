---
title: "aplikace SaaS aaaConfigure pro spolupráci B2B ve službě Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: c3f22f81567c04ac23ef2316c09de718ecb15d26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-saas-apps-for-b2b-collaboration"></a><span data-ttu-id="ec335-103">Konfigurace aplikací SaaS pro spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-103">Configure SaaS apps for B2B collaboration</span></span>

<span data-ttu-id="ec335-104">Spolupráce Azure Active Directory (Azure AD) s B2B pracuje s většinu aplikací, které se integrují s Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ec335-104">Azure Active Directory (Azure AD) B2B collaboration works with most apps that integrate with Azure AD.</span></span> <span data-ttu-id="ec335-105">V této části jsme provede pokyny ke konfiguraci některých oblíbených aplikací SaaS pro použití s Azure AD s B2B.</span><span class="sxs-lookup"><span data-stu-id="ec335-105">In this section, we walk through instructions for configuring some popular SaaS apps for use with Azure AD B2B.</span></span>

<span data-ttu-id="ec335-106">Předtím, než se podíváte na konkrétní aplikaci pokyny, zde jsou některé zásady:</span><span class="sxs-lookup"><span data-stu-id="ec335-106">Before you look at app-specific instructions, here are some rules of thumb:</span></span>

* <span data-ttu-id="ec335-107">Pro většinu aplikací hello nastavení uživatele musí toohappen ručně.</span><span class="sxs-lookup"><span data-stu-id="ec335-107">For most of hello apps, user setup needs toohappen manually.</span></span> <span data-ttu-id="ec335-108">To znamená uživatelé musí být vytvořeny ručně v aplikaci hello také.</span><span class="sxs-lookup"><span data-stu-id="ec335-108">That is, users must be created manually in hello app as well.</span></span>

* <span data-ttu-id="ec335-109">Pro aplikace, které podporují automatické nastavení, jako je Dropbox jsou vytvořeny samostatné pozvánek z aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ec335-109">For apps that support automatic setup, such as Dropbox, separate invitations are created from hello apps.</span></span> <span data-ttu-id="ec335-110">Uživatelé musí být jisti tooaccept každý pozvánku.</span><span class="sxs-lookup"><span data-stu-id="ec335-110">Users must be sure tooaccept each invitation.</span></span>

* <span data-ttu-id="ec335-111">V hello uživatelské atributy toomitigate všechny problémy s disk profilu pozměnění uživatele (UPD) v uživatele typu Host, vždy nastavená **uživatelský identifikátor** příliš**user.mail**.</span><span class="sxs-lookup"><span data-stu-id="ec335-111">In hello user attributes, toomitigate any issues with mangled user profile disk (UPD) in guest users, always set **User Identifier** too**user.mail**.</span></span>


## <a name="dropbox-business"></a><span data-ttu-id="ec335-112">Obchodní dropbox</span><span class="sxs-lookup"><span data-stu-id="ec335-112">Dropbox Business</span></span>

<span data-ttu-id="ec335-113">tooenable toosign uživatele pomocí svého účtu organizace, je nutné ručně nakonfigurovat Dropbox obchodní toouse Azure AD jako zprostředkovatele identity Security (Assertion Markup Language SAML).</span><span class="sxs-lookup"><span data-stu-id="ec335-113">tooenable users toosign in using their organization account, you must manually configure Dropbox Business toouse Azure AD as a Security Assertion Markup Language (SAML) identity provider.</span></span> <span data-ttu-id="ec335-114">Pokud obchodní Dropbox nebyl nakonfigurovaný toodo tedy nemůže výzvu nebo jinak povolit toosign uživatelů v používání služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ec335-114">If Dropbox Business has not been configured toodo so, it cannot prompt or otherwise allow users toosign in using Azure AD.</span></span>

1. <span data-ttu-id="ec335-115">tooadd hello Dropbox obchodní aplikace do služby Azure AD, vyberte **podnikové aplikace, které** v levém podokně text hello a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ec335-115">tooadd hello Dropbox Business app into Azure AD, select **Enterprise applications** in hello left pane, and then click **Add**.</span></span>

  ![tlačítko "Přidat" Hello na stránce aplikace hello Enterprise](media/active-directory-b2b-configure-saas-apps/add-dropbox.png)

2. <span data-ttu-id="ec335-117">V hello **přidat aplikaci** okno, zadejte **dropbox** v hello vyhledávacího pole a pak vyberte **Dropbox pro firmy** v seznamu výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="ec335-117">In hello **Add an application** window, enter **dropbox** in hello search box, and then select **Dropbox for Business** in hello results list.</span></span>

  ![Vyhledejte "dropbox" na hello přidání stránky aplikace](media/active-directory-b2b-configure-saas-apps/add-app-dialog.png)

3. <span data-ttu-id="ec335-119">Na hello **jednotného přihlašování** vyberte **jednotného přihlašování** v levém podokně text hello a potom zadejte **user.mail** v hello **uživatelský identifikátor** pole.</span><span class="sxs-lookup"><span data-stu-id="ec335-119">On hello **Single sign-on** page, select **Single sign-on** in hello left pane, and then enter **user.mail** in hello **User Identifier** box.</span></span> <span data-ttu-id="ec335-120">(Je nastavený jako UPN ve výchozím nastavení.)</span><span class="sxs-lookup"><span data-stu-id="ec335-120">(It's set as UPN by default.)</span></span>

  ![Konfigurace jednotného přihlašování pro aplikace hello](media/active-directory-b2b-configure-saas-apps/configure-app-sso.png)

4. <span data-ttu-id="ec335-122">Vyberte toodownload hello certifikát toouse pro konfiguraci Dropbox, **konfigurace DropBox**a potom vyberte **SAML jeden znak na adresu URL služby** v seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="ec335-122">toodownload hello certificate toouse for Dropbox configuration, select **Configure DropBox**, and then select **SAML Single Sign On Service URL** in hello list.</span></span>

  ![Stahování hello certifikát pro Dropbox konfigurace](media/active-directory-b2b-configure-saas-apps/download-certificate.png)

5. <span data-ttu-id="ec335-124">Přihlášení tooDropbox s hello přihlašování adresu URL z hello **jednotného přihlašování** stránky.</span><span class="sxs-lookup"><span data-stu-id="ec335-124">Sign in tooDropbox with hello sign-on URL from hello **Single sign-on** page.</span></span>

  ![stránku Hello přihlášení Dropbox](media/active-directory-b2b-configure-saas-apps/sign-in-to-dropbox.png)

6. <span data-ttu-id="ec335-126">V nabídce hello vyberte **konzoly pro správu**.</span><span class="sxs-lookup"><span data-stu-id="ec335-126">On hello menu, select **Admin Console**.</span></span>

  ![Hello "Konzoly pro správu" odkaz v nabídce Dropbox hello](media/active-directory-b2b-configure-saas-apps/dropbox-menu.png)

7. <span data-ttu-id="ec335-128">V hello **ověřování** dialogové okno, vyberte **Další**, nahrajte certifikát hello a pak na hello **přihlašovací adresa URL** zadejte adresu URL hello SAML jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="ec335-128">In hello **Authentication** dialog box, select **More**, upload hello certificate and then, in hello **Sign in URL** box, enter hello SAML single sign-on URL.</span></span>

  !["Informace" odkaz v dialogovém okně ověřování hello sbalené Hello](media/active-directory-b2b-configure-saas-apps/dropbox-auth-01.png)

  ![Hello "Přihlášení v URL" v hello rozšířit ověřování, dialogové okno](media/active-directory-b2b-configure-saas-apps/paste-single-sign-on-URL.png)

8. <span data-ttu-id="ec335-131">nastavení automatického uživatele tooconfigure v hello portál Azure, vyberte **zřizování** v levém podokně hello vyberte **automatické** v hello **režimu zřizování** a potom vyberte **Autorizovat**.</span><span class="sxs-lookup"><span data-stu-id="ec335-131">tooconfigure automatic user setup in hello Azure portal, select **Provisioning** in hello left pane, select **Automatic** in hello **Provisioning Mode** box, and then select **Authorize**.</span></span>

  ![Konfiguraci zřizování automatické uživatelů v hello portálu Azure](media/active-directory-b2b-configure-saas-apps/set-up-automatic-provisioning.png)

<span data-ttu-id="ec335-133">Po hosta nebo člen uživatelů nastavili v aplikaci hello Dropbox, obdrží samostatné pozvánku z Dropbox.</span><span class="sxs-lookup"><span data-stu-id="ec335-133">After guest or member users have been set up in hello Dropbox app, they receive a separate invitation from Dropbox.</span></span> <span data-ttu-id="ec335-134">toouse Dropbox jednotné přihlašování, budou pozvaní uživatelé musí přijmout pozvánku hello kliknutím na odkaz v ní.</span><span class="sxs-lookup"><span data-stu-id="ec335-134">toouse Dropbox single sign-on, invitees must accept hello invitation by clicking a link in it.</span></span>

## <a name="box"></a><span data-ttu-id="ec335-135">Box</span><span class="sxs-lookup"><span data-stu-id="ec335-135">Box</span></span>
<span data-ttu-id="ec335-136">Uživatelé tooauthenticate pole uživatele typu Host ke svému účtu Azure AD můžete povolit pomocí federace, která je založená na hello protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="ec335-136">You can enable users tooauthenticate Box guest users with their Azure AD account by using federation that's based on hello SAML protocol.</span></span> <span data-ttu-id="ec335-137">V tomto postupu nahrajte tooBox.com metadat.</span><span class="sxs-lookup"><span data-stu-id="ec335-137">In this procedure, you upload metadata tooBox.com.</span></span>

1. <span data-ttu-id="ec335-138">Přidáte aplikaci Box hello z hello podnikové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ec335-138">Add hello Box app from hello enterprise apps.</span></span>

2. <span data-ttu-id="ec335-139">Nakonfigurujte jednotné přihlašování v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="ec335-139">Configure single sign-on in hello following order:</span></span>

  ![Konfigurace pole jednotné přihlašování](media/active-directory-b2b-configure-saas-apps/configure-box-sso.png)

 <span data-ttu-id="ec335-141">a.</span><span class="sxs-lookup"><span data-stu-id="ec335-141">a.</span></span> <span data-ttu-id="ec335-142">V hello **přihlásit na adrese URL** pole, zkontrolujte, že hello přihlašovací adresa URL je správně nastavená pro pole v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ec335-142">In hello **Sign on URL** box, ensure that hello sign-on URL is set appropriately for Box in hello Azure portal.</span></span> <span data-ttu-id="ec335-143">Tato adresa URL je adresa URL hello klienta služby Box.com.</span><span class="sxs-lookup"><span data-stu-id="ec335-143">This URL is hello URL of your Box.com tenant.</span></span> <span data-ttu-id="ec335-144">Měl by splňovat zásady vytváření názvů hello *https://.box.com*.</span><span class="sxs-lookup"><span data-stu-id="ec335-144">It should follow hello naming convention *https://.box.com*.</span></span>  
 <span data-ttu-id="ec335-145">Hello **identifikátor** doporučení se netýká toothis aplikace, ale stále se zobrazí jako povinné pole.</span><span class="sxs-lookup"><span data-stu-id="ec335-145">hello **Identifier** does not apply toothis app, but it still appears as a mandatory field.</span></span>

 <span data-ttu-id="ec335-146">b.</span><span class="sxs-lookup"><span data-stu-id="ec335-146">b.</span></span> <span data-ttu-id="ec335-147">V hello **uživatelský identifikátor** zadejte **user.mail** (pro jednotné přihlašování pro účet hosta).</span><span class="sxs-lookup"><span data-stu-id="ec335-147">In hello **User identifier** box, enter **user.mail** (for SSO for guest accounts).</span></span>

 <span data-ttu-id="ec335-148">c.</span><span class="sxs-lookup"><span data-stu-id="ec335-148">c.</span></span> <span data-ttu-id="ec335-149">V části **SAML podpisový certifikát**, klikněte na tlačítko **vytvořit nový certifikát**.</span><span class="sxs-lookup"><span data-stu-id="ec335-149">Under **SAML Signing Certificate**, click **Create new certificate**.</span></span>

 <span data-ttu-id="ec335-150">d.</span><span class="sxs-lookup"><span data-stu-id="ec335-150">d.</span></span> <span data-ttu-id="ec335-151">toobegin konfigurace vaší toouse Box.com klienta Azure AD jako zprostředkovatele identity, stažení souboru metadat hello a pak ho uložte tooyour místní disk.</span><span class="sxs-lookup"><span data-stu-id="ec335-151">toobegin configuring your Box.com tenant toouse Azure AD as an identity provider, download hello metadata file and then save it tooyour local drive.</span></span>

 <span data-ttu-id="ec335-152">e.</span><span class="sxs-lookup"><span data-stu-id="ec335-152">e.</span></span> <span data-ttu-id="ec335-153">Předat dál hello metadata souboru toohello pole tým podpory, který nakonfiguruje jednotné přihlašování pro vás.</span><span class="sxs-lookup"><span data-stu-id="ec335-153">Forward hello metadata file toohello Box support team, which configures single sign-on for you.</span></span>

3. <span data-ttu-id="ec335-154">Nastavení automatického uživatele Azure AD, v levém podokně hello vyberte **zřizování**a potom vyberte **Authorize**.</span><span class="sxs-lookup"><span data-stu-id="ec335-154">For Azure AD automatic user setup, in hello left pane, select **Provisioning**, and then select **Authorize**.</span></span>

  ![Autorizovat tooBox tooconnect Azure AD](media/active-directory-b2b-configure-saas-apps/auth-azure-ad-to-connect-to-box.png)

<span data-ttu-id="ec335-156">Jako budou pozvaní uživatelé Dropbox musí pozvaným uživatelům pole uplatnit jejich pozvání aplikaci Box hello.</span><span class="sxs-lookup"><span data-stu-id="ec335-156">Like Dropbox invitees, Box invitees must redeem their invitation from hello Box app.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec335-157">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ec335-157">Next steps</span></span>

<span data-ttu-id="ec335-158">Najdete v následujících článcích na spolupráci Azure AD B2B hello:</span><span class="sxs-lookup"><span data-stu-id="ec335-158">See hello following articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ec335-159">Co je spolupráce B2B ve službě Azure AD?</span><span class="sxs-lookup"><span data-stu-id="ec335-159">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ec335-160">Vlastnosti uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-160">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="ec335-161">Přidání role uživatele tooa spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-161">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="ec335-162">Delegovat pozvánek spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-162">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="ec335-163">Dynamické skupiny a spolupráci B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-163">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="ec335-164">Kód spolupráce B2B a ukázky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec335-164">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="ec335-165">Tokeny uživatele spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-165">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="ec335-166">Deklarace uživatele spolupráce B2B mapování</span><span class="sxs-lookup"><span data-stu-id="ec335-166">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="ec335-167">Externí sdílení Office 365</span><span class="sxs-lookup"><span data-stu-id="ec335-167">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="ec335-168">Aktuální omezení spolupráce B2B</span><span class="sxs-lookup"><span data-stu-id="ec335-168">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
