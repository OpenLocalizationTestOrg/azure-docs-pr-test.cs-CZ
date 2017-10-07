---
title: "Kurz: Azure Active Directory integration Procore jednotného přihlašování k | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Procore jednotné přihlašování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bb918617f18ba3f44ddde469e6fc411977752f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a><span data-ttu-id="f1e74-103">Kurz: Azure Active Directory integrace s Procore jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-103">Tutorial: Azure Active Directory integration with Procore SSO</span></span>

<span data-ttu-id="f1e74-104">V tomto kurzu zjistíte, jak toointegrate Procore jednotné přihlašování s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f1e74-104">In this tutorial, you learn how toointegrate Procore SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f1e74-105">Integrace Procore jednotné přihlašování s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f1e74-105">Integrating Procore SSO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f1e74-106">Můžete řídit ve službě Azure AD, který má přístup tooProcore jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-106">You can control in Azure AD who has access tooProcore SSO</span></span>
- <span data-ttu-id="f1e74-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooProcore jednotné přihlašování (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1e74-107">You can enable your users tooautomatically get signed-on tooProcore SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f1e74-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="f1e74-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="f1e74-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f1e74-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Procore SSO, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="f1e74-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f1e74-110">Prerequisites</span></span>

<span data-ttu-id="f1e74-111">tooconfigure integrace Azure AD pomocí Procore jednotného přihlašování, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="f1e74-111">tooconfigure Azure AD integration with Procore SSO, you need hello following items:</span></span>

- <span data-ttu-id="f1e74-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1e74-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f1e74-113">Procore SSO jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f1e74-113">A Procore SSO single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f1e74-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f1e74-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f1e74-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f1e74-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f1e74-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="f1e74-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="f1e74-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1e74-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f1e74-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f1e74-118">Scenario description</span></span>
<span data-ttu-id="f1e74-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f1e74-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f1e74-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f1e74-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f1e74-121">Přidání Procore jednotného přihlašování z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f1e74-121">Adding Procore SSO from hello gallery</span></span>
2. <span data-ttu-id="f1e74-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-procore-sso-from-hello-gallery"></a><span data-ttu-id="f1e74-123">Přidání Procore jednotného přihlašování z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="f1e74-123">Adding Procore SSO from hello gallery</span></span>
<span data-ttu-id="f1e74-124">tooconfigure hello integrace Procore přihlašování do služby Azure AD, je nutné tooadd Procore SSO hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="f1e74-124">tooconfigure hello integration of Procore SSO into Azure AD, you need tooadd Procore SSO from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f1e74-125">**tooadd Procore jednotného přihlašování z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f1e74-125">**tooadd Procore SSO from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1e74-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f1e74-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f1e74-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f1e74-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f1e74-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f1e74-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f1e74-133">Hello vyhledávacího pole zadejte **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-133">In hello search box, type **Procore SSO**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. <span data-ttu-id="f1e74-135">Na panelu výsledků hello vyberte **Procore jednotného přihlašování k**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1e74-135">In hello results panel, select **Procore SSO**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f1e74-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f1e74-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Procore jednotného přihlašování na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f1e74-138">In this section, you configure and test Azure AD single sign-on with Procore SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f1e74-139">Pro toowork jeden přihlašování Azure AD musí tooknow, co uživatel protějšku hello Procore SSO je tooa uživatelem ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f1e74-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Procore SSO is tooa user in Azure AD.</span></span> <span data-ttu-id="f1e74-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello Procore SSO musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="f1e74-140">In other words, a link relationship between an Azure AD user and hello related user in Procore SSO needs toobe established.</span></span>

<span data-ttu-id="f1e74-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** Procore sso.</span><span class="sxs-lookup"><span data-stu-id="f1e74-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Procore SSO.</span></span>

<span data-ttu-id="f1e74-142">tooconfigure a testování Azure AD jednotné přihlašování pomocí jednotného přihlašování služby Procore, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="f1e74-142">tooconfigure and test Azure AD single sign-on with Procore SSO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f1e74-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f1e74-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f1e74-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f1e74-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f1e74-145">**[Vytvoření zkušebního uživatele Procore SSO](#creating-a-procore-sso-test-user)**  -toohave protějšek Britta Simon v Procore jednotné přihlašování, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f1e74-145">**[Creating a Procore SSO test user](#creating-a-procore-sso-test-user)** - toohave a counterpart of Britta Simon in Procore SSO that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="f1e74-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f1e74-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f1e74-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="f1e74-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f1e74-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f1e74-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Procore jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f1e74-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Procore SSO application.</span></span>

<span data-ttu-id="f1e74-150">**tooconfigure Azure AD jednotné přihlašování s Procore jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f1e74-150">**tooconfigure Azure AD single sign-on with Procore SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1e74-151">V hello Azure Management portal na hello **Procore SSO** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-151">In hello Azure Management portal, on hello **Procore SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f1e74-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="f1e74-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. <span data-ttu-id="f1e74-155">Na hello **Procore jednotného přihlašování k doméně a adresy URL** část, hello uživatel nemá tooperform žádné kroky jako aplikace hello je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="f1e74-155">On hello **Procore SSO Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. <span data-ttu-id="f1e74-157">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f1e74-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. <span data-ttu-id="f1e74-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f1e74-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f1e74-161">Na hello **Procore Konfigurace jednotného přihlašování k** klikněte na tlačítko **Konfigurace jednotného přihlašování k Procore** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f1e74-161">On hello **Procore SSO Configuration** section, click **Configure Procore SSO** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f1e74-162">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f1e74-162">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. <span data-ttu-id="f1e74-164">tooconfigure jednotného přihlašování na **Procore SSO** straně, lokality procore společnosti tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="f1e74-164">tooconfigure single sign-on on **Procore SSO** side, login tooyour procore company site as an administrator.</span></span>

8. <span data-ttu-id="f1e74-165">Sada nástrojů rozevíracím hello dolů, klikněte na **správce** tooopen hello jednotného přihlašování k nastavení stránky.</span><span class="sxs-lookup"><span data-stu-id="f1e74-165">From hello toolbox drop down, click on **Admin** tooopen hello SSO settings page.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. <span data-ttu-id="f1e74-167">Vložit hello hodnoty v polích hello jako popsané níže-</span><span class="sxs-lookup"><span data-stu-id="f1e74-167">Paste hello values in hello boxes as described below-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    <span data-ttu-id="f1e74-169">a.</span><span class="sxs-lookup"><span data-stu-id="f1e74-169">a.</span></span> <span data-ttu-id="f1e74-170">V hello **jedné URL přihlašování na vystavitele** pole, vložte hello SAML Entity ID zkopírovaných z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f1e74-170">In hello **Single Sign On Issuer URL** box, paste hello SAML Entity ID copied from hello Azure portal.</span></span>

    <span data-ttu-id="f1e74-171">b.</span><span class="sxs-lookup"><span data-stu-id="f1e74-171">b.</span></span> <span data-ttu-id="f1e74-172">V hello **SAML přihlašovací na cílová adresa URL** pole, vložte hello SAML jeden přihlašování adresa URL služby zkopírovaných z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f1e74-172">In hello **SAML Sign On Target URL** box, paste hello SAML Single Sign-On Service URL copied from hello Azure portal.</span></span>

    <span data-ttu-id="f1e74-173">c.</span><span class="sxs-lookup"><span data-stu-id="f1e74-173">c.</span></span> <span data-ttu-id="f1e74-174">Nyní otevřete hello **soubor XML s metadaty** výše stažený z hello portál Azure a kopírovat hello certifikátu hello značky s názvem **certifikátu x 509**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-174">Now open hello **Metadata XML** downloaded above from hello Azure portal and copy hello certficate in hello tag named **X509Certificate**.</span></span> <span data-ttu-id="f1e74-175">Vložení hello zkopírují hello hodnotu **jednotné přihlašování x509 certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="f1e74-175">Paste hello copied value into hello **Single Sign On x509 Certificate** box.</span></span>

10. <span data-ttu-id="f1e74-176">Klikněte na **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-176">Click on **Save Changes**.</span></span>

11. <span data-ttu-id="f1e74-177">Po tato nastavení, musí toosend hello **název domény** (např **contoso.com**) prostřednictvím které přihlašujete Procore toohello [tým podpory Procore](https://support.procore.com/) a ty budou Aktivujte federované jednotné přihlašování pro tuto doménu.</span><span class="sxs-lookup"><span data-stu-id="f1e74-177">After these settings, you needs toosend hello **domain name** (e.g **contoso.com**) through which you are logging into Procore toohello [Procore Support team](https://support.procore.com/) and they will activate federated SSO for that domain.</span></span>

<!--### Next steps

tooensure users can sign-in tooProcore SSO after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooProcore SSO in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f1e74-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f1e74-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="f1e74-179">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f1e74-179">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f1e74-181">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f1e74-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1e74-182">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f1e74-182">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f1e74-184">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f1e74-184">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f1e74-186">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f1e74-186">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f1e74-188">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f1e74-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f1e74-190">a.</span><span class="sxs-lookup"><span data-stu-id="f1e74-190">a.</span></span> <span data-ttu-id="f1e74-191">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-191">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f1e74-192">b.</span><span class="sxs-lookup"><span data-stu-id="f1e74-192">b.</span></span> <span data-ttu-id="f1e74-193">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f1e74-193">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f1e74-194">c.</span><span class="sxs-lookup"><span data-stu-id="f1e74-194">c.</span></span> <span data-ttu-id="f1e74-195">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f1e74-196">d.</span><span class="sxs-lookup"><span data-stu-id="f1e74-196">d.</span></span> <span data-ttu-id="f1e74-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-197">Click **Create**.</span></span>
 
### <a name="creating-a-procore-sso-test-user"></a><span data-ttu-id="f1e74-198">Vytvoření zkušebního uživatele Procore jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-198">Creating a Procore SSO test user</span></span>

<span data-ttu-id="f1e74-199">Postupujte podle hello níže kroky toocreate Procore testovacího uživatele na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="f1e74-199">Please follow hello below steps toocreate a Procore test user on their side.</span></span>

1. <span data-ttu-id="f1e74-200">Lokalita procore společnosti tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="f1e74-200">Login tooyour procore company site as an administrator.</span></span>  

2. <span data-ttu-id="f1e74-201">Sada nástrojů rozevíracím hello dolů, klikněte na **Directory** stránky adresáře společnosti tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="f1e74-201">From hello toolbox drop down, click on **Directory** tooopen hello company directory page.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. <span data-ttu-id="f1e74-203">Klikněte na **přidat osobu** možnost tooopen hello formuláře a zadejte provést následující možnosti -</span><span class="sxs-lookup"><span data-stu-id="f1e74-203">Click on **Add a Person** option tooopen hello form and enter perform following options -</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    <span data-ttu-id="f1e74-205">a.</span><span class="sxs-lookup"><span data-stu-id="f1e74-205">a.</span></span> <span data-ttu-id="f1e74-206">V hello **křestní jméno** textovému poli, křestní jméno uživatele typu jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-206">In hello **First Name** textbox, type user's first name like **Britta**.</span></span>

    <span data-ttu-id="f1e74-207">b.</span><span class="sxs-lookup"><span data-stu-id="f1e74-207">b.</span></span> <span data-ttu-id="f1e74-208">V hello **příjmení** textovému poli, příjmení uživatele typu jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-208">In hello **Last name** textbox, type user's last name like **Simon**.</span></span>

    <span data-ttu-id="f1e74-209">c.</span><span class="sxs-lookup"><span data-stu-id="f1e74-209">c.</span></span> <span data-ttu-id="f1e74-210">V hello **e-mailovou adresu** textovému poli, typu uživatele e-mailovou adresu jako  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="f1e74-210">In hello **Email Address** textbox, type user's email address like **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="f1e74-211">d.</span><span class="sxs-lookup"><span data-stu-id="f1e74-211">d.</span></span> <span data-ttu-id="f1e74-212">Vyberte **šablona oprávnění** jako **později použít šablonu oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-212">Select **Permission Template** as **Apply Permission Template Later**.</span></span>

    <span data-ttu-id="f1e74-213">e.</span><span class="sxs-lookup"><span data-stu-id="f1e74-213">e.</span></span> <span data-ttu-id="f1e74-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-214">Click **Create**.</span></span>

4. <span data-ttu-id="f1e74-215">Zkontrolujte a aktualizujte hello podrobnosti pro hello nově přidali kontakt.</span><span class="sxs-lookup"><span data-stu-id="f1e74-215">Check and update hello details for hello newly added contact.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. <span data-ttu-id="f1e74-217">Klikněte na **uložit a odeslat Invitiation** (Pokud pozvání prostřednictvím e-mailu je vyžadována) nebo **Uložit** (uložit přímo) toocomplete hello uživatele registrace.</span><span class="sxs-lookup"><span data-stu-id="f1e74-217">Click on **Save and Send Invitiation** (if an invite through mail is required) or **Save** (Save directly) toocomplete hello user registration.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f1e74-219">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="f1e74-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f1e74-220">V této části povolíte jeho přístup tooProcore jednotné přihlašování, poskytněte Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f1e74-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooProcore SSO.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f1e74-222">**tooassign tooProcore Britta Simon jednotné přihlašování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="f1e74-222">**tooassign Britta Simon tooProcore SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="f1e74-223">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-223">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f1e74-225">V seznamu aplikace hello vyberte **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-225">In hello applications list, select **Procore SSO**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. <span data-ttu-id="f1e74-227">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f1e74-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f1e74-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f1e74-229">Click **Add** button.</span></span> <span data-ttu-id="f1e74-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f1e74-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f1e74-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="f1e74-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f1e74-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f1e74-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f1e74-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f1e74-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f1e74-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f1e74-235">Testing single sign-on</span></span>

<span data-ttu-id="f1e74-236">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f1e74-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f1e74-237">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="f1e74-237">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="f1e74-238">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="f1e74-238">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> <span data-ttu-id="f1e74-239">Když kliknete na dlaždici hello Procore jednotné přihlašování v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Procore jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1e74-239">When you click hello Procore SSO tile in hello Access Panel, you should get automatically signed-on tooyour Procore SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1e74-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f1e74-240">Additional resources</span></span>

* [<span data-ttu-id="f1e74-241">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f1e74-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f1e74-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f1e74-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

