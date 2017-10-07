---
title: 'Kurz: Azure Active Directory integrace s Teamphoria | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Teamphoria."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: f32be9742b76f7fe464036dadc108c62e4a787a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a><span data-ttu-id="6e4df-103">Kurz: Azure Active Directory integrace s Teamphoria</span><span class="sxs-lookup"><span data-stu-id="6e4df-103">Tutorial: Azure Active Directory integration with Teamphoria</span></span>

<span data-ttu-id="6e4df-104">V tomto kurzu zjistíte, jak toointegrate Teamphoria s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e4df-104">In this tutorial, you learn how toointegrate Teamphoria with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6e4df-105">Integrace Teamphoria s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6e4df-105">Integrating Teamphoria with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6e4df-106">Můžete řídit ve službě Azure AD, který má přístup tooTeamphoria</span><span class="sxs-lookup"><span data-stu-id="6e4df-106">You can control in Azure AD who has access tooTeamphoria</span></span>
- <span data-ttu-id="6e4df-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooTeamphoria (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e4df-107">You can enable your users tooautomatically get signed-on tooTeamphoria (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6e4df-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure hello</span><span class="sxs-lookup"><span data-stu-id="6e4df-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="6e4df-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6e4df-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Teamphoria, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="6e4df-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e4df-110">Prerequisites</span></span>

<span data-ttu-id="6e4df-111">Integrace služby Azure AD s Teamphoria tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6e4df-111">tooconfigure Azure AD integration with Teamphoria, you need hello following items:</span></span>

- <span data-ttu-id="6e4df-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e4df-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6e4df-113">Teamphoria jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6e4df-113">A Teamphoria single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6e4df-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e4df-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6e4df-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6e4df-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6e4df-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="6e4df-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="6e4df-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6e4df-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6e4df-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6e4df-118">Scenario description</span></span>
<span data-ttu-id="6e4df-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e4df-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6e4df-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6e4df-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6e4df-121">Přidání Teamphoria z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6e4df-121">Adding Teamphoria from hello gallery</span></span>
2. <span data-ttu-id="6e4df-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4df-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamphoria-from-hello-gallery"></a><span data-ttu-id="6e4df-123">Přidání Teamphoria z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6e4df-123">Adding Teamphoria from hello gallery</span></span>
<span data-ttu-id="6e4df-124">tooconfigure hello integrace Teamphoria do Azure AD, je nutné tooadd Teamphoria hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6e4df-124">tooconfigure hello integration of Teamphoria into Azure AD, you need tooadd Teamphoria from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6e4df-125">**tooadd Teamphoria z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4df-125">**tooadd Teamphoria from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4df-126">V hello  **[portálu pro správu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e4df-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6e4df-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6e4df-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6e4df-131">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e4df-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6e4df-133">Hello vyhledávacího pole zadejte **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-133">In hello search box, type **Teamphoria**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. <span data-ttu-id="6e4df-135">Na panelu výsledků hello vyberte **Teamphoria**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6e4df-135">In hello results panel, select **Teamphoria**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6e4df-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4df-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6e4df-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Teamphoria podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6e4df-138">In this section, you configure and test Azure AD single sign-on with Teamphoria based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6e4df-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Teamphoria je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e4df-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Teamphoria is tooa user in Azure AD.</span></span> <span data-ttu-id="6e4df-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Teamphoria musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6e4df-140">In other words, a link relationship between an Azure AD user and hello related user in Teamphoria needs toobe established.</span></span>

<span data-ttu-id="6e4df-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="6e4df-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Teamphoria.</span></span>

<span data-ttu-id="6e4df-142">tooconfigure a testu Azure AD jednotné přihlašování s Teamphoria, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6e4df-142">tooconfigure and test Azure AD single sign-on with Teamphoria, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6e4df-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6e4df-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6e4df-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e4df-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6e4df-145">**[Vytvoření zkušebního uživatele Teamphoria](#creating-a-teamphoria-test-user)**  -toohave protějšek Britta Simon v Teamphoria, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e4df-145">**[Creating a Teamphoria test user](#creating-a-teamphoria-test-user)** - toohave a counterpart of Britta Simon in Teamphoria that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="6e4df-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4df-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6e4df-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6e4df-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6e4df-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4df-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6e4df-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="6e4df-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Teamphoria application.</span></span>

<span data-ttu-id="6e4df-150">**tooconfigure Azure AD jednotné přihlašování s Teamphoria, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4df-150">**tooconfigure Azure AD single sign-on with Teamphoria, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4df-151">V hello Azure Management portal na hello **Teamphoria** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-151">In hello Azure Management portal, on hello **Teamphoria** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6e4df-153">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="6e4df-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. <span data-ttu-id="6e4df-155">Na hello **Teamphoria domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6e4df-155">On hello **Teamphoria Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    <span data-ttu-id="6e4df-157">a.</span><span class="sxs-lookup"><span data-stu-id="6e4df-157">a.</span></span> <span data-ttu-id="6e4df-158">V hello **přihlašovací adresa URL** textovému poli, typ hello adresu URL pomocí hello následující vzoru:`https://<sub-domain>.teamphoria.com/login`</span><span class="sxs-lookup"><span data-stu-id="6e4df-158">In hello **Sign-on URL** textbox, type hello URL using hello following pattern: `https://<sub-domain>.teamphoria.com/login`</span></span>  

    > [!NOTE] 
    > <span data-ttu-id="6e4df-159">Upozorňujeme, že tyto nejsou hello skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6e4df-159">Please note that these are not hello real values.</span></span> <span data-ttu-id="6e4df-160">Máte tooupdate tyto hodnoty s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4df-160">You have tooupdate these values with hello actual Sign-On URL.</span></span> <span data-ttu-id="6e4df-161">Obraťte se na [tým podpory Teamphoria klienta](https://www.teamphoria.com/) tooget hello přihlašování URL.</span><span class="sxs-lookup"><span data-stu-id="6e4df-161">Contact [Teamphoria Client support team](https://www.teamphoria.com/) tooget hello Sign-on URL.</span></span> 

4. <span data-ttu-id="6e4df-162">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte certifikát hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6e4df-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. <span data-ttu-id="6e4df-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e4df-164">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6e4df-166">Na hello **Teamphoria konfigurace** klikněte na tlačítko **konfigurace Teamphoria** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6e4df-166">On hello **Teamphoria Configuration** section, click **Configure Teamphoria** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6e4df-167">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6e4df-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. <span data-ttu-id="6e4df-169">tooconfigure jednotného přihlašování na **Teamphoria** straně, aplikace Teamphoria tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="6e4df-169">tooconfigure single sign-on on **Teamphoria** side, Login tooyour Teamphoria application as an administrator.</span></span>

8. <span data-ttu-id="6e4df-170">Přejděte příliš**nastavení správce** v levém panelu nástrojů hello a v části hello hello karta Konfigurace klikněte na možnost **jeden přihlašování** okno Konfigurace jednotného přihlašování k tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="6e4df-170">Go too**ADMIN SETTINGS** option in hello left toolbar and under hello hello Configure Tab click on **SINGLE SIGN-ON** tooopen hello SSO configuration window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. <span data-ttu-id="6e4df-172">Klikněte na **přidat nového poskytovatele IDENTITY** možnost v hello pravém horním rohu tooopen hello formuláře pro přidání hello nastavení pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4df-172">Click on **ADD NEW IDENTITY PROVIDER** option in hello top right corner tooopen hello form for adding hello settings for SSO.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. <span data-ttu-id="6e4df-174">Zadejte podrobnosti hello v polích text hello, jak je popsáno níže-</span><span class="sxs-lookup"><span data-stu-id="6e4df-174">Enter hello details in hello fields as described below-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    <span data-ttu-id="6e4df-176">a.</span><span class="sxs-lookup"><span data-stu-id="6e4df-176">a.</span></span> <span data-ttu-id="6e4df-177">**Zobrazit název** : Zadejte zobrazovaný název modulu plug-in hello hello na stránky pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="6e4df-177">**DISPLAY NAME** : Enter hello display name of hello plugin on hello admin page.</span></span>

    <span data-ttu-id="6e4df-178">b.</span><span class="sxs-lookup"><span data-stu-id="6e4df-178">b.</span></span> <span data-ttu-id="6e4df-179">**Název TLAČÍTKA** : název hello hello kartu, která se zobrazí na přihlašovací stránku hello pro přihlašování pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e4df-179">**BUTTON NAME** : hello name of hello tab which will display on hello login page for logging in via SSO.</span></span>

    <span data-ttu-id="6e4df-180">c.</span><span class="sxs-lookup"><span data-stu-id="6e4df-180">c.</span></span> <span data-ttu-id="6e4df-181">**CERTIFIKÁT** : Otevřete hello certifikát předtím stáhli z portálu Azure v poznámkovém bloku, kopírovat obsah hello hello hello stejné a vložte ji sem hello pole.</span><span class="sxs-lookup"><span data-stu-id="6e4df-181">**CERTIFICATE** : Open hello Certificate downloaded earlier from hello Azure portal in notepad, copy hello contents of hello same and paste it here in hello box.</span></span>

    <span data-ttu-id="6e4df-182">d.</span><span class="sxs-lookup"><span data-stu-id="6e4df-182">d.</span></span> <span data-ttu-id="6e4df-183">**VSTUPNÍ bod** : vložení hello **SAML jeden přihlašování adresa URL služby** zkopírovat starší hello formulář portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e4df-183">**ENTRY POINT** : Paste hello **SAML Single Sign-On Service URL** copied earlier form hello Azure portal.</span></span>

    <span data-ttu-id="6e4df-184">e.</span><span class="sxs-lookup"><span data-stu-id="6e4df-184">e.</span></span> <span data-ttu-id="6e4df-185">Přepněte možnost hello příliš**ON** a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-185">Switch hello option too**ON** and click on **SAVE**.</span></span> 

<!--### Next steps

tooensure users can sign-in tooTeamphoria after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooTeamphoria in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6e4df-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e4df-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="6e4df-187">Hello cílem této části je toocreate testovacího uživatele na portálu pro správu Azure hello názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e4df-187">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6e4df-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4df-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4df-190">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e4df-190">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6e4df-192">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6e4df-192">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6e4df-194">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4df-194">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6e4df-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e4df-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6e4df-198">a.</span><span class="sxs-lookup"><span data-stu-id="6e4df-198">a.</span></span> <span data-ttu-id="6e4df-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6e4df-200">b.</span><span class="sxs-lookup"><span data-stu-id="6e4df-200">b.</span></span> <span data-ttu-id="6e4df-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6e4df-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6e4df-202">c.</span><span class="sxs-lookup"><span data-stu-id="6e4df-202">c.</span></span> <span data-ttu-id="6e4df-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6e4df-204">d.</span><span class="sxs-lookup"><span data-stu-id="6e4df-204">d.</span></span> <span data-ttu-id="6e4df-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-205">Click **Create**.</span></span>
 
### <a name="creating-a-teamphoria-test-user"></a><span data-ttu-id="6e4df-206">Vytvoření zkušebního uživatele Teamphoria</span><span class="sxs-lookup"><span data-stu-id="6e4df-206">Creating a Teamphoria test user</span></span>

<span data-ttu-id="6e4df-207">V pořadí tooenable Azure AD Uživatelé toolog do Teamphoria musí být zřízená do Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="6e4df-207">In order tooenable Azure AD users toolog into Teamphoria, they must be provisioned into Teamphoria.</span></span> <span data-ttu-id="6e4df-208">V případě hello Teamphoria zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="6e4df-208">In hello case of Teamphoria, provisioning is a manual task.</span></span>

<span data-ttu-id="6e4df-209">**tooprovision uživatelské účty, provádět hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6e4df-209">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4df-210">Přihlaste se tooyour Teamphoria společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="6e4df-210">Log in tooyour Teamphoria company site as an administrator.</span></span>

2. <span data-ttu-id="6e4df-211">Klikněte na **správce** nastavení na levém panelu nástrojů hello a v části hello **SPRAVOVAT** kartě kliknutím na **uživatelé** stránky pro správu hello tooopen pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e4df-211">Click on **ADMIN** settings on hello left toolbar and under hello **MANAGE** tab Click on **USERS** tooopen hello admin page for users.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. <span data-ttu-id="6e4df-213">Klikněte na hello **RUČNÍ POZVAT** možnost.</span><span class="sxs-lookup"><span data-stu-id="6e4df-213">Click on hello **MANUAL INVITE** option.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. <span data-ttu-id="6e4df-215">Na této stránce proveďte následující akce.</span><span class="sxs-lookup"><span data-stu-id="6e4df-215">On this page, perform following action.</span></span> 
    
    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    <span data-ttu-id="6e4df-217">a.</span><span class="sxs-lookup"><span data-stu-id="6e4df-217">a.</span></span> <span data-ttu-id="6e4df-218">V hello **e-MAILOVOU adresu** textovému poli, hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6e4df-218">In hello **EMAIL ADDRESS** textbox, hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6e4df-219">b.</span><span class="sxs-lookup"><span data-stu-id="6e4df-219">b.</span></span> <span data-ttu-id="6e4df-220">V hello **KŘESTNÍ jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-220">In hello **FIRST NAME** textbox, type **Britta**.</span></span>

    <span data-ttu-id="6e4df-221">c.</span><span class="sxs-lookup"><span data-stu-id="6e4df-221">c.</span></span> <span data-ttu-id="6e4df-222">V hello **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-222">In hello **LAST NAME** textbox, type **Simon**.</span></span>

    <span data-ttu-id="6e4df-223">d.</span><span class="sxs-lookup"><span data-stu-id="6e4df-223">d.</span></span> <span data-ttu-id="6e4df-224">Klikněte na tlačítko **pozvání 1 uživatele**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-224">Click **INVITE 1 USER**.</span></span> <span data-ttu-id="6e4df-225">Uživatel potřebuje tooaccept hello pozvání tooget vytvořené v systému hello.</span><span class="sxs-lookup"><span data-stu-id="6e4df-225">User needs tooaccept hello invite tooget created in hello system.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6e4df-226">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6e4df-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6e4df-227">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte tooTeamphoria svůj přístup.</span><span class="sxs-lookup"><span data-stu-id="6e4df-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooTeamphoria.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6e4df-229">**tooassign Britta Simon tooTeamphoria, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6e4df-229">**tooassign Britta Simon tooTeamphoria, perform hello following steps:**</span></span>

1. <span data-ttu-id="6e4df-230">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-230">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6e4df-232">V seznamu aplikace hello vyberte **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-232">In hello applications list, select **Teamphoria**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. <span data-ttu-id="6e4df-234">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6e4df-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6e4df-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e4df-236">Click **Add** button.</span></span> <span data-ttu-id="6e4df-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4df-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6e4df-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6e4df-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6e4df-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4df-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6e4df-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e4df-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6e4df-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e4df-242">Testing single sign-on</span></span>

<span data-ttu-id="6e4df-243">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6e4df-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6e4df-244">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="6e4df-244">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="6e4df-245">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="6e4df-245">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6e4df-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6e4df-246">Additional resources</span></span>

* [<span data-ttu-id="6e4df-247">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e4df-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6e4df-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6e4df-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

