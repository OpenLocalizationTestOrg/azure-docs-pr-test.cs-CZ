---
title: 'Kurz: Azure Active Directory integrace s Samanage | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Samanage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c8edc29f113b8088438618a731e97c0f4f155b9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a><span data-ttu-id="9ff6a-103">Kurz: Azure Active Directory integrace s Samanage</span><span class="sxs-lookup"><span data-stu-id="9ff6a-103">Tutorial: Azure Active Directory integration with Samanage</span></span>

<span data-ttu-id="9ff6a-104">V tomto kurzu zjistíte, jak toointegrate Samanage s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ff6a-104">In this tutorial, you learn how toointegrate Samanage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ff6a-105">Integrace Samanage s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-105">Integrating Samanage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9ff6a-106">Můžete řídit ve službě Azure AD, který má přístup tooSamanage</span><span class="sxs-lookup"><span data-stu-id="9ff6a-106">You can control in Azure AD who has access tooSamanage</span></span>
- <span data-ttu-id="9ff6a-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSamanage (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ff6a-107">You can enable your users tooautomatically get signed-on tooSamanage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9ff6a-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9ff6a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9ff6a-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ff6a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ff6a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ff6a-110">Prerequisites</span></span>

<span data-ttu-id="9ff6a-111">Integrace služby Azure AD s Samanage tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-111">tooconfigure Azure AD integration with Samanage, you need hello following items:</span></span>

- <span data-ttu-id="9ff6a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ff6a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ff6a-113">Samanage jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9ff6a-113">A Samanage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ff6a-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ff6a-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ff6a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ff6a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ff6a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ff6a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9ff6a-118">Scenario description</span></span>
<span data-ttu-id="9ff6a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ff6a-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ff6a-121">Přidání Samanage z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9ff6a-121">Adding Samanage from hello gallery</span></span>
2. <span data-ttu-id="9ff6a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ff6a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-samanage-from-hello-gallery"></a><span data-ttu-id="9ff6a-123">Přidání Samanage z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9ff6a-123">Adding Samanage from hello gallery</span></span>
<span data-ttu-id="9ff6a-124">tooconfigure hello integrace Samanage do Azure AD, je nutné tooadd Samanage hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-124">tooconfigure hello integration of Samanage into Azure AD, you need tooadd Samanage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9ff6a-125">**tooadd Samanage z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ff6a-125">**tooadd Samanage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ff6a-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9ff6a-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9ff6a-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9ff6a-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9ff6a-133">Hello vyhledávacího pole zadejte **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-133">In hello search box, type **Samanage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. <span data-ttu-id="9ff6a-135">Na panelu výsledků hello vyberte **Samanage**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-135">In hello results panel, select **Samanage**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9ff6a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ff6a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9ff6a-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Samanage podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9ff6a-138">In this section, you configure and test Azure AD single sign-on with Samanage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9ff6a-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Samanage je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Samanage is tooa user in Azure AD.</span></span> <span data-ttu-id="9ff6a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Samanage musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-140">In other words, a link relationship between an Azure AD user and hello related user in Samanage needs toobe established.</span></span>

<span data-ttu-id="9ff6a-141">V Samanage, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-141">In Samanage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9ff6a-142">tooconfigure a testu Azure AD jednotné přihlašování s Samanage, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-142">tooconfigure and test Azure AD single sign-on with Samanage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9ff6a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9ff6a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ff6a-145">**[Vytvoření zkušebního uživatele Samanage](#creating-a-samanage-test-user)**  -toohave protějšek Britta Simon v Samanage, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-145">**[Creating a Samanage test user](#creating-a-samanage-test-user)** - toohave a counterpart of Britta Simon in Samanage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ff6a-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ff6a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9ff6a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ff6a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9ff6a-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Samanage.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Samanage application.</span></span>

<span data-ttu-id="9ff6a-150">**tooconfigure Azure AD jednotné přihlašování s Samanage, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ff6a-150">**tooconfigure Azure AD single sign-on with Samanage, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ff6a-151">V portálu Azure, na hello hello **Samanage** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-151">In hello Azure portal, on hello **Samanage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9ff6a-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. <span data-ttu-id="9ff6a-155">Na hello **Samanage domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-155">On hello **Samanage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    <span data-ttu-id="9ff6a-157">a.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-157">a.</span></span> <span data-ttu-id="9ff6a-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<Company Name>.samanage.com/saml_login/<Company Name>`</span><span class="sxs-lookup"><span data-stu-id="9ff6a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<Company Name>.samanage.com/saml_login/<Company Name>`</span></span>

    <span data-ttu-id="9ff6a-159">b.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-159">b.</span></span> <span data-ttu-id="9ff6a-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<Company Name>.samanage.com`</span><span class="sxs-lookup"><span data-stu-id="9ff6a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<Company Name>.samanage.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ff6a-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-161">These values are not real.</span></span> <span data-ttu-id="9ff6a-162">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátor, který je vysvětlen později v kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-162">Update these values with hello actual Sign-on URL and Identifier, which is explained later in hello tutorial.</span></span> <span data-ttu-id="9ff6a-163">Obraťte se na podrobnosti [tým podpory Samanage klienta](https://www.samanage.com/support).</span><span class="sxs-lookup"><span data-stu-id="9ff6a-163">For more details contact [Samanage Client support team](https://www.samanage.com/support).</span></span>    
 
4. <span data-ttu-id="9ff6a-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. <span data-ttu-id="9ff6a-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9ff6a-168">Na hello **Samanage konfigurace** klikněte na tlačítko **konfigurace Samanage** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-168">On hello **Samanage Configuration** section, click **Configure Samanage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9ff6a-169">Kopírování hello **Sign-Out adresy URL a SAML Entity ID** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9ff6a-169">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. <span data-ttu-id="9ff6a-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Samanage.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-171">In a different web browser window, log into your Samanage company site as an administrator.</span></span>

8. <span data-ttu-id="9ff6a-172">Klikněte na tlačítko **řídicí panel** a vyberte **instalační program** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-172">Click **Dashboard** and select **Setup** in left navigation pane.</span></span>
   
    <span data-ttu-id="9ff6a-173">![Řídicí panel](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "řídicí panel")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-173">![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")</span></span>

9. <span data-ttu-id="9ff6a-174">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-174">Click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="9ff6a-175">![Jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-175">![Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")</span></span>

10. <span data-ttu-id="9ff6a-176">Přejděte příliš**přihlášení pomocí SAML** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-176">Navigate too**Login using SAML** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="9ff6a-177">![Přihlášení pomocí SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "přihlášení pomocí SAML")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-177">![Login using SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "Login using SAML")</span></span>
 
    <span data-ttu-id="9ff6a-178">a.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-178">a.</span></span> <span data-ttu-id="9ff6a-179">Klikněte na tlačítko **povolit jednotné přihlašování s SAML**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-179">Click **Enable Single Sign-On with SAML**.</span></span>  
 
    <span data-ttu-id="9ff6a-180">b.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-180">b.</span></span> <span data-ttu-id="9ff6a-181">V hello **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu hello **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-181">In hello **Identity Provider URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>    
 
    <span data-ttu-id="9ff6a-182">c.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-182">c.</span></span> <span data-ttu-id="9ff6a-183">Potvrďte hello **přihlašovací adresa URL** odpovídá hello **přihlašovací adresa URL** z **Samanage domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-183">Confirm hello **Login URL** matches hello **Sign On URL** of **Samanage Domain and URLs** section in Azure portal.</span></span>
 
    <span data-ttu-id="9ff6a-184">d.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-184">d.</span></span> <span data-ttu-id="9ff6a-185">V hello **adresy URL odhlašovací** textovému poli, zadejte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-185">In hello **Logout URL** textbox, enter hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="9ff6a-186">e.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-186">e.</span></span> <span data-ttu-id="9ff6a-187">V hello **SAML vystavitele** textovému poli, zadejte id aplikace hello URI nastavit v zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-187">In hello **SAML Issuer** textbox, type hello app id URI set in your identity provider.</span></span>
 
    <span data-ttu-id="9ff6a-188">f.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-188">f.</span></span> <span data-ttu-id="9ff6a-189">Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v poznámkovém bloku hello kopírování obsahu ho do schránky a pak ji vložit toohello **vložit vašeho poskytovatele identit x.509 certifikátu níže** textové pole.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-189">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **Paste your Identity Provider x.509 Certificate below** textbox.</span></span>
 
    <span data-ttu-id="9ff6a-190">g.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-190">g.</span></span> <span data-ttu-id="9ff6a-191">Klikněte na tlačítko **vytváření uživatelů, pokud neexistují v Samanage**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-191">Click **Create users if they do not exist in Samanage**.</span></span>
 
    <span data-ttu-id="9ff6a-192">h.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-192">h.</span></span> <span data-ttu-id="9ff6a-193">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-193">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="9ff6a-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="9ff6a-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9ff6a-195">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9ff6a-196">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ff6a-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9ff6a-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ff6a-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="9ff6a-198">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9ff6a-200">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ff6a-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ff6a-201">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9ff6a-203">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9ff6a-205">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9ff6a-207">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ff6a-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9ff6a-209">a.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-209">a.</span></span> <span data-ttu-id="9ff6a-210">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ff6a-211">b.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-211">b.</span></span> <span data-ttu-id="9ff6a-212">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9ff6a-213">c.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-213">c.</span></span> <span data-ttu-id="9ff6a-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9ff6a-215">d.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-215">d.</span></span> <span data-ttu-id="9ff6a-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-216">Click **Create**.</span></span>
 
### <a name="creating-a-samanage-test-user"></a><span data-ttu-id="9ff6a-217">Vytvoření zkušebního uživatele Samanage</span><span class="sxs-lookup"><span data-stu-id="9ff6a-217">Creating a Samanage test user</span></span>

<span data-ttu-id="9ff6a-218">Uživatelé toolog tooenable Azure AD v tooSamanage, se musí být zřízená do Samanage.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-218">tooenable Azure AD users toolog in tooSamanage, they must be provisioned into Samanage.</span></span>  
<span data-ttu-id="9ff6a-219">V případě hello Samanage zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-219">In hello case of Samanage, provisioning is a manual task.</span></span>

<span data-ttu-id="9ff6a-220">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ff6a-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ff6a-221">Přihlaste se k serveru vaší společnosti Samanage jako správce.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-221">Log into your Samanage company site as an administrator.</span></span>

2. <span data-ttu-id="9ff6a-222">Klikněte na tlačítko **řídicí panel** a vyberte **instalační program** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-222">Click **Dashboard** and select **Setup** in left navigation pan.</span></span>
   
    <span data-ttu-id="9ff6a-223">![Instalační program](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-223">![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")</span></span>

3. <span data-ttu-id="9ff6a-224">Klikněte na tlačítko hello **uživatelé** karta</span><span class="sxs-lookup"><span data-stu-id="9ff6a-224">Click hello **Users** tab</span></span>
   
    <span data-ttu-id="9ff6a-225">![Uživatelé](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-225">![Users](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "Users")</span></span>

4. <span data-ttu-id="9ff6a-226">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-226">Click **New User**.</span></span>
   
    <span data-ttu-id="9ff6a-227">![Nový uživatel](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-227">![New User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "New User")</span></span>

5. <span data-ttu-id="9ff6a-228">Typ hello **název** a hello **e-mailovou adresu** účtu Azure Active Directory tooprovision a klikněte na **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-228">Type hello **Name** and hello **Email Address** of an Azure Active Directory account you want tooprovision and click **Create user**.</span></span>
   
    <span data-ttu-id="9ff6a-229">![Vytvoření uživatele](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="9ff6a-229">![Create User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "Create User")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="9ff6a-230">Držitel účtu Azure Active Directory Hello bude dostávat e-mailu a postupujte podle tooconfirm odkaz svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-230">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span> <span data-ttu-id="9ff6a-231">Můžete použít všechny ostatní Samanage uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Samanage tooprovision Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-231">You can use any other Samanage user account creation tools or APIs provided by Samanage tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9ff6a-232">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9ff6a-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9ff6a-233">V této části povolíte tak, že udělíte přístup tooSamanage toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSamanage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9ff6a-235">**tooassign Britta Simon tooSamanage, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9ff6a-235">**tooassign Britta Simon tooSamanage, perform hello following steps:**</span></span>

1. <span data-ttu-id="9ff6a-236">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9ff6a-238">V seznamu aplikace hello vyberte **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-238">In hello applications list, select **Samanage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. <span data-ttu-id="9ff6a-240">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9ff6a-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-242">Click **Add** button.</span></span> <span data-ttu-id="9ff6a-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9ff6a-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9ff6a-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ff6a-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9ff6a-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ff6a-248">Testing single sign-on</span></span>

<span data-ttu-id="9ff6a-249">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-249">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9ff6a-250">Když kliknete na dlaždici Samanage hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Samanage aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ff6a-250">When you click hello Samanage tile in hello Access Panel, you should get automatically signed-on tooyour Samanage application.</span></span>
<span data-ttu-id="9ff6a-251">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9ff6a-251">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ff6a-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9ff6a-252">Additional resources</span></span>

* [<span data-ttu-id="9ff6a-253">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9ff6a-253">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ff6a-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9ff6a-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

