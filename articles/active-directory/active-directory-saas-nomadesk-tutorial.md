---
title: 'Kurz: Azure Active Directory integrace s Nomadesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Nomadesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d261b776-b48e-45f0-9722-0297adefabb8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 9aa55ba6106ea24f556217cfe84d21d7415d9c78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadesk"></a><span data-ttu-id="3a5b5-103">Kurz: Azure Active Directory integrace s Nomadesk</span><span class="sxs-lookup"><span data-stu-id="3a5b5-103">Tutorial: Azure Active Directory integration with Nomadesk</span></span>

<span data-ttu-id="3a5b5-104">V tomto kurzu zjistíte, jak toointegrate Nomadesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3a5b5-104">In this tutorial, you learn how toointegrate Nomadesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3a5b5-105">Integrace Nomadesk s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-105">Integrating Nomadesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3a5b5-106">Můžete řídit ve službě Azure AD, který má přístup tooNomadesk</span><span class="sxs-lookup"><span data-stu-id="3a5b5-106">You can control in Azure AD who has access tooNomadesk</span></span>
- <span data-ttu-id="3a5b5-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooNomadesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3a5b5-107">You can enable your users tooautomatically get signed-on tooNomadesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3a5b5-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3a5b5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3a5b5-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3a5b5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a5b5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3a5b5-110">Prerequisites</span></span>

<span data-ttu-id="3a5b5-111">Integrace služby Azure AD s Nomadesk tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-111">tooconfigure Azure AD integration with Nomadesk, you need hello following items:</span></span>

- <span data-ttu-id="3a5b5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3a5b5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3a5b5-113">Nomadesk jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3a5b5-113">A Nomadesk single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3a5b5-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3a5b5-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3a5b5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3a5b5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3a5b5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3a5b5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3a5b5-118">Scenario description</span></span>
<span data-ttu-id="3a5b5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3a5b5-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3a5b5-121">Přidání Nomadesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3a5b5-121">Adding Nomadesk from hello gallery</span></span>
2. <span data-ttu-id="3a5b5-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3a5b5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nomadesk-from-hello-gallery"></a><span data-ttu-id="3a5b5-123">Přidání Nomadesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="3a5b5-123">Adding Nomadesk from hello gallery</span></span>
<span data-ttu-id="3a5b5-124">tooconfigure hello integrace Nomadesk do Azure AD, je nutné tooadd Nomadesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-124">tooconfigure hello integration of Nomadesk into Azure AD, you need tooadd Nomadesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3a5b5-125">**tooadd Nomadesk z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3a5b5-125">**tooadd Nomadesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a5b5-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3a5b5-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3a5b5-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3a5b5-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3a5b5-133">Hello vyhledávacího pole zadejte **Nomadesk**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-133">In hello search box, type **Nomadesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_search.png)

5. <span data-ttu-id="3a5b5-135">Na panelu výsledků hello vyberte **Nomadesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-135">In hello results panel, select **Nomadesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3a5b5-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3a5b5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3a5b5-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Nomadesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="3a5b5-138">In this section, you configure and test Azure AD single sign-on with Nomadesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3a5b5-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Nomadesk je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Nomadesk is tooa user in Azure AD.</span></span> <span data-ttu-id="3a5b5-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Nomadesk musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-140">In other words, a link relationship between an Azure AD user and hello related user in Nomadesk needs toobe established.</span></span>

<span data-ttu-id="3a5b5-141">V Nomadesk, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-141">In Nomadesk, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3a5b5-142">tooconfigure a testu Azure AD jednotné přihlašování s Nomadesk, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-142">tooconfigure and test Azure AD single sign-on with Nomadesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3a5b5-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3a5b5-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3a5b5-145">**[Vytvoření zkušebního uživatele Nomadesk](#creating-a-nomadesk-test-user)**  -toohave protějšek Britta Simon v Nomadesk, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-145">**[Creating a Nomadesk test user](#creating-a-nomadesk-test-user)** - toohave a counterpart of Britta Simon in Nomadesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3a5b5-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3a5b5-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3a5b5-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3a5b5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3a5b5-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Nomadesk.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Nomadesk application.</span></span>

<span data-ttu-id="3a5b5-150">**tooconfigure Azure AD jednotné přihlašování s Nomadesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3a5b5-150">**tooconfigure Azure AD single sign-on with Nomadesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a5b5-151">V portálu Azure, na hello hello **Nomadesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-151">In hello Azure portal, on hello **Nomadesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3a5b5-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_samlbase.png)

3. <span data-ttu-id="3a5b5-155">Na hello **Nomadesk domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-155">On hello **Nomadesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_url.png)

    <span data-ttu-id="3a5b5-157">a.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-157">a.</span></span> <span data-ttu-id="3a5b5-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://mynomadesk.com/logon/saml/<TENANTID>`</span><span class="sxs-lookup"><span data-stu-id="3a5b5-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mynomadesk.com/logon/saml/<TENANTID>`</span></span>

    <span data-ttu-id="3a5b5-159">b.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-159">b.</span></span> <span data-ttu-id="3a5b5-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://secure.nomadesk.com/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="3a5b5-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://secure.nomadesk.com/saml/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3a5b5-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-161">These values are not real.</span></span> <span data-ttu-id="3a5b5-162">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="3a5b5-163">Obraťte se na [tým podpory Nomadesk klienta](mailto:support@nomadesk.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-163">Contact [Nomadesk Client support team](mailto:support@nomadesk.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="3a5b5-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_certificate.png) 

5. <span data-ttu-id="3a5b5-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nomadesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3a5b5-168">Na hello **Nomadesk konfigurace** klikněte na tlačítko **konfigurace Nomadesk** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-168">On hello **Nomadesk Configuration** section, click **Configure Nomadesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3a5b5-169">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="3a5b5-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_configure.png) 

7. <span data-ttu-id="3a5b5-171">tooconfigure jednotného přihlašování na **Nomadesk** straně, je nutné stáhnout hello toosend **certifikát**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[tým podpory Nomadesk](mailto:support@nomadesk.com).</span><span class="sxs-lookup"><span data-stu-id="3a5b5-171">tooconfigure single sign-on on **Nomadesk** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Nomadesk support team](mailto:support@nomadesk.com).</span></span> <span data-ttu-id="3a5b5-172">Nastavují hello toohave tato nastavení jednotného přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="3a5b5-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="3a5b5-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3a5b5-174">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3a5b5-175">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3a5b5-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3a5b5-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3a5b5-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="3a5b5-177">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3a5b5-179">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3a5b5-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a5b5-180">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3a5b5-182">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3a5b5-184">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3a5b5-186">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3a5b5-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-nomadesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3a5b5-188">a.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-188">a.</span></span> <span data-ttu-id="3a5b5-189">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3a5b5-190">b.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-190">b.</span></span> <span data-ttu-id="3a5b5-191">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3a5b5-192">c.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-192">c.</span></span> <span data-ttu-id="3a5b5-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3a5b5-194">d.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-194">d.</span></span> <span data-ttu-id="3a5b5-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-195">Click **Create**.</span></span>
 
### <a name="creating-a-nomadesk-test-user"></a><span data-ttu-id="3a5b5-196">Vytvoření zkušebního uživatele Nomadesk</span><span class="sxs-lookup"><span data-stu-id="3a5b5-196">Creating a Nomadesk test user</span></span>

<span data-ttu-id="3a5b5-197">Hello cílem této části je toocreate volal Britta Simon v Nomadesk uživatele.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-197">hello objective of this section is toocreate a user called Britta Simon in Nomadesk.</span></span> <span data-ttu-id="3a5b5-198">Nomadesk podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-198">Nomadesk supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="3a5b5-199">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-199">There is no action item for you in this section.</span></span> <span data-ttu-id="3a5b5-200">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Nomadesk.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-200">A new user is created during an attempt tooaccess Nomadesk if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="3a5b5-201">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Nomadesk](mailto:support@nomadesk.com).</span><span class="sxs-lookup"><span data-stu-id="3a5b5-201">If you need toocreate a user manually, you need toocontact hello [Nomadesk support team](mailto:support@nomadesk.com).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3a5b5-202">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="3a5b5-202">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3a5b5-203">V této části povolíte tak, že udělíte přístup tooNomadesk toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-203">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNomadesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3a5b5-205">**tooassign Britta Simon tooNomadesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="3a5b5-205">**tooassign Britta Simon tooNomadesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a5b5-206">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-206">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3a5b5-208">V seznamu aplikace hello vyberte **Nomadesk**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-208">In hello applications list, select **Nomadesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-nomadesk-tutorial/tutorial_nomadesk_app.png) 

3. <span data-ttu-id="3a5b5-210">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-210">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3a5b5-212">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-212">Click **Add** button.</span></span> <span data-ttu-id="3a5b5-213">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3a5b5-215">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-215">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3a5b5-216">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3a5b5-217">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3a5b5-218">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3a5b5-218">Testing single sign-on</span></span>

<span data-ttu-id="3a5b5-219">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-219">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3a5b5-220">Když kliknete na dlaždici Nomadesk hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Nomadesk aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a5b5-220">When you click hello Nomadesk tile in hello Access Panel,  you should get automatically signed-on tooyour Nomadesk application.</span></span>
<span data-ttu-id="3a5b5-221">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3a5b5-221">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3a5b5-222">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3a5b5-222">Additional resources</span></span>

* [<span data-ttu-id="3a5b5-223">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3a5b5-223">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3a5b5-224">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3a5b5-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadesk-tutorial/tutorial_general_203.png

