---
title: 'Kurz: Azure Active Directory integrace s SD elementy | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SD elementy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 77949e41beb541c9fe8147b1eb2e7995e05bd753
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a><span data-ttu-id="2f35c-103">Kurz: Azure Active Directory integrace s SD elementy</span><span class="sxs-lookup"><span data-stu-id="2f35c-103">Tutorial: Azure Active Directory integration with SD Elements</span></span>

<span data-ttu-id="2f35c-104">V tomto kurzu zjistíte, jak toointegrate SD elementů pomocí Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2f35c-104">In this tutorial, you learn how toointegrate SD Elements with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2f35c-105">Integrace s Azure AD SD elementy poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2f35c-105">Integrating SD Elements with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2f35c-106">Můžete řídit ve službě Azure AD, který má přístup tooSD elementy</span><span class="sxs-lookup"><span data-stu-id="2f35c-106">You can control in Azure AD who has access tooSD Elements</span></span>
- <span data-ttu-id="2f35c-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSD elementy (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f35c-107">You can enable your users tooautomatically get signed-on tooSD Elements (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2f35c-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2f35c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2f35c-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2f35c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f35c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2f35c-110">Prerequisites</span></span>

<span data-ttu-id="2f35c-111">tooconfigure integrace Azure AD SD elementy, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="2f35c-111">tooconfigure Azure AD integration with SD Elements, you need hello following items:</span></span>

- <span data-ttu-id="2f35c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f35c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2f35c-113">Elementy SD jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2f35c-113">A SD Elements single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2f35c-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f35c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2f35c-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2f35c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2f35c-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2f35c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2f35c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2f35c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2f35c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2f35c-118">Scenario description</span></span>
<span data-ttu-id="2f35c-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f35c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2f35c-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2f35c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2f35c-121">Přidávání elementů SD z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2f35c-121">Adding SD Elements from hello gallery</span></span>
2. <span data-ttu-id="2f35c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f35c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sd-elements-from-hello-gallery"></a><span data-ttu-id="2f35c-123">Přidávání elementů SD z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="2f35c-123">Adding SD Elements from hello gallery</span></span>
<span data-ttu-id="2f35c-124">tooconfigure hello integrace SD elementů do Azure AD, je nutné tooadd SD elementy hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2f35c-124">tooconfigure hello integration of SD Elements into Azure AD, you need tooadd SD Elements from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2f35c-125">**tooadd SD elementy z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2f35c-125">**tooadd SD Elements from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f35c-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2f35c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2f35c-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2f35c-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2f35c-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f35c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2f35c-133">Hello vyhledávacího pole zadejte **SD elementy**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-133">In hello search box, type **SD Elements**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_search.png)

5. <span data-ttu-id="2f35c-135">Na panelu výsledků hello vyberte **SD elementy**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f35c-135">In hello results panel, select **SD Elements**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2f35c-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f35c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2f35c-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s SD elementy podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2f35c-138">In this section, you configure and test Azure AD single sign-on with SD Elements based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2f35c-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v elementech SD je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f35c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SD Elements is tooa user in Azure AD.</span></span> <span data-ttu-id="2f35c-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v elementech SD musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="2f35c-140">In other words, a link relationship between an Azure AD user and hello related user in SD Elements needs toobe established.</span></span>

<span data-ttu-id="2f35c-141">V elementech SD přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="2f35c-141">In SD Elements, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2f35c-142">tooconfigure a testu Azure AD jednotné přihlašování s SD elementy, je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="2f35c-142">tooconfigure and test Azure AD single sign-on with SD Elements, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2f35c-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="2f35c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2f35c-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f35c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2f35c-145">**[Vytvoření zkušebního uživatele SD elementy](#creating-a-sd-elements-test-user)**  -toohave protějšek Britta Simon v elementech SD, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="2f35c-145">**[Creating a SD Elements test user](#creating-a-sd-elements-test-user)** - toohave a counterpart of Britta Simon in SD Elements that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2f35c-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f35c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2f35c-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="2f35c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2f35c-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f35c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2f35c-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci SD elementy.</span><span class="sxs-lookup"><span data-stu-id="2f35c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SD Elements application.</span></span>

<span data-ttu-id="2f35c-150">**tooconfigure Azure AD jednotného přihlašování u elementů na SD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2f35c-150">**tooconfigure Azure AD single sign-on with SD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f35c-151">V portálu Azure, na hello hello **SD elementy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-151">In hello Azure portal, on hello **SD Elements** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2f35c-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f35c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_samlbase.png)

3. <span data-ttu-id="2f35c-155">Na hello **SD elementy domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2f35c-155">On hello **SD Elements Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_url.png)

    <span data-ttu-id="2f35c-157">a.</span><span class="sxs-lookup"><span data-stu-id="2f35c-157">a.</span></span> <span data-ttu-id="2f35c-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.sdelements.com/sso/saml2/metadata`</span><span class="sxs-lookup"><span data-stu-id="2f35c-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.sdelements.com/sso/saml2/metadata`</span></span>

    <span data-ttu-id="2f35c-159">b.</span><span class="sxs-lookup"><span data-stu-id="2f35c-159">b.</span></span> <span data-ttu-id="2f35c-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<tenantname>.sdelements.com/sso/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="2f35c-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<tenantname>.sdelements.com/sso/saml2/acs/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2f35c-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2f35c-161">These values are not real.</span></span> <span data-ttu-id="2f35c-162">Tyto hodnoty aktualizujte pomocí hello skutečné identifikátor dotazů a odpovědí adresy URL.</span><span class="sxs-lookup"><span data-stu-id="2f35c-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="2f35c-163">Obraťte se na [tým podpory SD elementy](mailto:support@sdelements.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2f35c-163">Contact [SD Elements support team](mailto:support@sdelements.com) tooget these values.</span></span>

4. <span data-ttu-id="2f35c-164">Elementy SD aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="2f35c-164">SD Elements application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2f35c-165">Nakonfigurujte hello následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f35c-165">Configure hello following claims for this application.</span></span> <span data-ttu-id="2f35c-166">Můžete spravovat hello hodnoty těchto atributů z hello **"Atribut uživatele"** karta aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="2f35c-166">You can manage hello values of these attributes from hello **"User Attribute"** tab of hello application.</span></span> <span data-ttu-id="2f35c-167">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="2f35c-167">hello following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_attribute.png)

5. <span data-ttu-id="2f35c-169">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, nakonfigurovat atribut tokenu SAML, jak je znázorněno v bitové kopii hello a provést hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f35c-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span> 

    | <span data-ttu-id="2f35c-170">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2f35c-170">Attribute Name</span></span> | <span data-ttu-id="2f35c-171">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2f35c-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="2f35c-172">E-mailu</span><span class="sxs-lookup"><span data-stu-id="2f35c-172">email</span></span> |<span data-ttu-id="2f35c-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="2f35c-173">user.mail</span></span> |
    | <span data-ttu-id="2f35c-174">FirstName</span><span class="sxs-lookup"><span data-stu-id="2f35c-174">firstname</span></span> |<span data-ttu-id="2f35c-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="2f35c-175">user.givenname</span></span> |
    | <span data-ttu-id="2f35c-176">Příjmení</span><span class="sxs-lookup"><span data-stu-id="2f35c-176">lastname</span></span> |<span data-ttu-id="2f35c-177">User.Surname</span><span class="sxs-lookup"><span data-stu-id="2f35c-177">user.surname</span></span> |

    <span data-ttu-id="2f35c-178">a.</span><span class="sxs-lookup"><span data-stu-id="2f35c-178">a.</span></span> <span data-ttu-id="2f35c-179">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f35c-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_officespace_05.png)

    <span data-ttu-id="2f35c-182">b.</span><span class="sxs-lookup"><span data-stu-id="2f35c-182">b.</span></span> <span data-ttu-id="2f35c-183">V hello **název** textovému poli, název atributu pro typ hello zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2f35c-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="2f35c-184">c.</span><span class="sxs-lookup"><span data-stu-id="2f35c-184">c.</span></span> <span data-ttu-id="2f35c-185">Z hello **hodnotu** seznamu, hodnota atributu hello typ zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="2f35c-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="2f35c-186">d.</span><span class="sxs-lookup"><span data-stu-id="2f35c-186">d.</span></span> <span data-ttu-id="2f35c-187">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-187">Click **Ok**.</span></span>
 
6. <span data-ttu-id="2f35c-188">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2f35c-188">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_certificate.png) 

7. <span data-ttu-id="2f35c-190">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f35c-190">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2f35c-192">Na hello **SD prvky konfigurace** klikněte na tlačítko **konfigurace elementy SD** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2f35c-192">On hello **SD Elements Configuration** section, click **Configure SD Elements** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2f35c-193">Kopírování hello **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="2f35c-193">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_configure.png)

9. <span data-ttu-id="2f35c-195">tooget jednotné přihlašování povoleno, obraťte se na vaše [tým podpory SD elementy](mailto:support@sdelements.com) a poskytnout soubor stažený certifikát hello.</span><span class="sxs-lookup"><span data-stu-id="2f35c-195">tooget single sign-on enabled, contact your [SD Elements support team](mailto:support@sdelements.com) and provide them with hello downloaded certificate file.</span></span> 

10. <span data-ttu-id="2f35c-196">V okně jiný prohlížeč klienta SD elementy tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="2f35c-196">In a different browser window, sign-on tooyour SD Elements tenant as an administrator.</span></span>

11. <span data-ttu-id="2f35c-197">V nabídce hello hello nahoře, klikněte na tlačítko **systému**a potom **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-197">In hello menu on hello top, click **System**, and then **Single Sign-on**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 

12. <span data-ttu-id="2f35c-199">Na hello **nastavení jednotného přihlašování** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2f35c-199">On hello **Single Sign-On Settings** dialog, perform hello following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    <span data-ttu-id="2f35c-201">a.</span><span class="sxs-lookup"><span data-stu-id="2f35c-201">a.</span></span> <span data-ttu-id="2f35c-202">Jako **typ jednotného přihlašování k**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-202">As **SSO Type**, select **SAML**.</span></span>
   
    <span data-ttu-id="2f35c-203">b.</span><span class="sxs-lookup"><span data-stu-id="2f35c-203">b.</span></span> <span data-ttu-id="2f35c-204">V hello **ID Entity zprostředkovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2f35c-204">In hello **Identity Provider Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="2f35c-205">c.</span><span class="sxs-lookup"><span data-stu-id="2f35c-205">c.</span></span> <span data-ttu-id="2f35c-206">V hello **Identity zprostředkovatele-služby přihlášení** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2f35c-206">In hello **Identity Provider Single Sign-On Service** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span> 
   
    <span data-ttu-id="2f35c-207">d.</span><span class="sxs-lookup"><span data-stu-id="2f35c-207">d.</span></span> <span data-ttu-id="2f35c-208">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-208">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2f35c-209">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="2f35c-209">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2f35c-210">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="2f35c-210">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2f35c-211">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2f35c-211">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2f35c-212">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f35c-212">Creating an Azure AD test user</span></span>
<span data-ttu-id="2f35c-213">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="2f35c-213">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2f35c-215">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2f35c-215">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f35c-216">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2f35c-216">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2f35c-218">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-218">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2f35c-220">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="2f35c-220">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2f35c-222">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f35c-222">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2f35c-224">a.</span><span class="sxs-lookup"><span data-stu-id="2f35c-224">a.</span></span> <span data-ttu-id="2f35c-225">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-225">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2f35c-226">b.</span><span class="sxs-lookup"><span data-stu-id="2f35c-226">b.</span></span> <span data-ttu-id="2f35c-227">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2f35c-227">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2f35c-228">c.</span><span class="sxs-lookup"><span data-stu-id="2f35c-228">c.</span></span> <span data-ttu-id="2f35c-229">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-229">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2f35c-230">d.</span><span class="sxs-lookup"><span data-stu-id="2f35c-230">d.</span></span> <span data-ttu-id="2f35c-231">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-231">Click **Create**.</span></span>
 
### <a name="creating-a-sd-elements-test-user"></a><span data-ttu-id="2f35c-232">Vytvoření zkušebního uživatele SD elementy</span><span class="sxs-lookup"><span data-stu-id="2f35c-232">Creating a SD Elements test user</span></span>

<span data-ttu-id="2f35c-233">Hello cílem této části je toocreate uživatel volal Britta Simon v SD elementy.</span><span class="sxs-lookup"><span data-stu-id="2f35c-233">hello objective of this section is toocreate a user called Britta Simon in SD Elements.</span></span> <span data-ttu-id="2f35c-234">V případě hello elementů SD vytváření SD elementy uživatelů je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="2f35c-234">In hello case of SD Elements, creating SD Elements users is a manual task.</span></span>

<span data-ttu-id="2f35c-235">**toocreate Britta Simon v elementech SD, proveďte hello následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f35c-235">**toocreate Britta Simon in SD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f35c-236">V okně prohlížeče webové, lokality společnosti SD elementy tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="2f35c-236">In a web browser window, sign-on tooyour SD Elements company site as an administrator.</span></span>

2. <span data-ttu-id="2f35c-237">V nabídce hello hello nahoře, klikněte na tlačítko **Správa uživatelů**a potom **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-237">In hello menu on hello top, click **User Management**, and then **Users**.</span></span>
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 

3. <span data-ttu-id="2f35c-239">Klikněte na tlačítko **přidat nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-239">Click **Add New User**.</span></span>
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png)
 
4. <span data-ttu-id="2f35c-241">Na hello **přidat nové uživatele** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="2f35c-241">On hello **Add New User** dialog, perform hello following steps:</span></span>
   
    ![Vytvoření zkušebního uživatele SD elementy](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    <span data-ttu-id="2f35c-243">a.</span><span class="sxs-lookup"><span data-stu-id="2f35c-243">a.</span></span> <span data-ttu-id="2f35c-244">V hello **e-mailu** textovému poli, zadejte e-mailu hello uživatele jako  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2f35c-244">In hello **E-mail** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="2f35c-245">b.</span><span class="sxs-lookup"><span data-stu-id="2f35c-245">b.</span></span> <span data-ttu-id="2f35c-246">V hello **křestní jméno** textovému poli, zadejte hello křestní jméno uživatele jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-246">In hello **First Name** textbox, enter hello first name of user like **Britta**.</span></span>
   
    <span data-ttu-id="2f35c-247">c.</span><span class="sxs-lookup"><span data-stu-id="2f35c-247">c.</span></span> <span data-ttu-id="2f35c-248">V hello **příjmení** textovému poli, zadejte příjmení uživatele jako hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-248">In hello **Last Name** textbox, enter hello last name of user like **Simon**.</span></span>
   
    <span data-ttu-id="2f35c-249">d.</span><span class="sxs-lookup"><span data-stu-id="2f35c-249">d.</span></span> <span data-ttu-id="2f35c-250">Jako **Role**, vyberte **uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-250">As **Role**, select **User**.</span></span> 
   
    <span data-ttu-id="2f35c-251">e.</span><span class="sxs-lookup"><span data-stu-id="2f35c-251">e.</span></span> <span data-ttu-id="2f35c-252">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-252">Click **Create User**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2f35c-253">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2f35c-253">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2f35c-254">V této části povolíte Britta Simon toouse Azure jednotného přihlašování k udělení přístupu tooSD elementy.</span><span class="sxs-lookup"><span data-stu-id="2f35c-254">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSD Elements.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2f35c-256">**tooassign Britta Simon tooSD prvky, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="2f35c-256">**tooassign Britta Simon tooSD Elements, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f35c-257">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-257">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2f35c-259">V seznamu aplikace hello vyberte **SD elementy**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-259">In hello applications list, select **SD Elements**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sd-elements-tutorial/tutorial_sdelements_app.png) 

3. <span data-ttu-id="2f35c-261">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2f35c-261">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2f35c-263">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f35c-263">Click **Add** button.</span></span> <span data-ttu-id="2f35c-264">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f35c-264">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2f35c-266">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="2f35c-266">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2f35c-267">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f35c-267">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2f35c-268">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f35c-268">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2f35c-269">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f35c-269">Testing single sign-on</span></span>

<span data-ttu-id="2f35c-270">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2f35c-270">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
  
<span data-ttu-id="2f35c-271">Po kliknutí na tlačítko hello SD elementy dlaždici v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SD elementy aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f35c-271">When you click hello SD Elements tile in hello Access Panel, you should get automatically signed-on tooyour SD Elements application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f35c-272">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2f35c-272">Additional resources</span></span>

* [<span data-ttu-id="2f35c-273">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2f35c-273">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2f35c-274">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2f35c-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png

