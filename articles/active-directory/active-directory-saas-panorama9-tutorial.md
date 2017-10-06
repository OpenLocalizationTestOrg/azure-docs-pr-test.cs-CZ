---
title: 'Kurz: Azure Active Directory integrace s Panorama9 | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Panorama9."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 548fb6434d920e076db98a0193f8dfdf8a958a91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a><span data-ttu-id="8ef00-103">Kurz: Azure Active Directory integrace s Panorama9</span><span class="sxs-lookup"><span data-stu-id="8ef00-103">Tutorial: Azure Active Directory integration with Panorama9</span></span>

<span data-ttu-id="8ef00-104">V tomto kurzu zjistíte, jak toointegrate Panorama9 s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8ef00-104">In this tutorial, you learn how toointegrate Panorama9 with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ef00-105">Integrace Panorama9 s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8ef00-105">Integrating Panorama9 with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8ef00-106">Můžete řídit ve službě Azure AD, který má přístup tooPanorama9</span><span class="sxs-lookup"><span data-stu-id="8ef00-106">You can control in Azure AD who has access tooPanorama9</span></span>
- <span data-ttu-id="8ef00-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPanorama9 (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ef00-107">You can enable your users tooautomatically get signed-on tooPanorama9 (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ef00-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8ef00-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8ef00-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ef00-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ef00-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8ef00-110">Prerequisites</span></span>

<span data-ttu-id="8ef00-111">Integrace služby Azure AD s Panorama9 tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8ef00-111">tooconfigure Azure AD integration with Panorama9, you need hello following items:</span></span>

- <span data-ttu-id="8ef00-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ef00-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ef00-113">Panorama9 jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8ef00-113">A Panorama9 single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8ef00-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ef00-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8ef00-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8ef00-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ef00-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8ef00-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8ef00-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ef00-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8ef00-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8ef00-118">Scenario description</span></span>
<span data-ttu-id="8ef00-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8ef00-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ef00-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8ef00-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ef00-121">Přidání Panorama9 z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8ef00-121">Adding Panorama9 from hello gallery</span></span>
2. <span data-ttu-id="8ef00-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ef00-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panorama9-from-hello-gallery"></a><span data-ttu-id="8ef00-123">Přidání Panorama9 z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8ef00-123">Adding Panorama9 from hello gallery</span></span>
<span data-ttu-id="8ef00-124">tooconfigure hello integrace Panorama9 do Azure AD, je nutné tooadd Panorama9 hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8ef00-124">tooconfigure hello integration of Panorama9 into Azure AD, you need tooadd Panorama9 from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8ef00-125">**tooadd Panorama9 z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ef00-125">**tooadd Panorama9 from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ef00-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ef00-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ef00-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8ef00-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8ef00-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ef00-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8ef00-133">Hello vyhledávacího pole zadejte **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-133">In hello search box, type **Panorama9**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. <span data-ttu-id="8ef00-135">Na panelu výsledků hello vyberte **Panorama9**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ef00-135">In hello results panel, select **Panorama9**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ef00-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ef00-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="8ef00-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Panorama9 podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="8ef00-138">In this section, you configure and test Azure AD single sign-on with Panorama9 based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8ef00-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Panorama9 je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8ef00-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Panorama9 is tooa user in Azure AD.</span></span> <span data-ttu-id="8ef00-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Panorama9 musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8ef00-140">In other words, a link relationship between an Azure AD user and hello related user in Panorama9 needs toobe established.</span></span>

<span data-ttu-id="8ef00-141">V Panorama9, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="8ef00-141">In Panorama9, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8ef00-142">tooconfigure a testu Azure AD jednotné přihlašování s Panorama9, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="8ef00-142">tooconfigure and test Azure AD single sign-on with Panorama9, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8ef00-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8ef00-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8ef00-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ef00-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ef00-145">**[Vytvoření zkušebního uživatele Panorama9](#creating-a-panorama9-test-user)**  -toohave protějšek Britta Simon v Panorama9, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ef00-145">**[Creating a Panorama9 test user](#creating-a-panorama9-test-user)** - toohave a counterpart of Britta Simon in Panorama9 that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8ef00-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ef00-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ef00-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8ef00-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ef00-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ef00-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ef00-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Panorama9.</span><span class="sxs-lookup"><span data-stu-id="8ef00-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Panorama9 application.</span></span>

<span data-ttu-id="8ef00-150">**tooconfigure Azure AD jednotné přihlašování s Panorama9, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ef00-150">**tooconfigure Azure AD single sign-on with Panorama9, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ef00-151">V portálu Azure, na hello hello **Panorama9** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-151">In hello Azure portal, on hello **Panorama9** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8ef00-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ef00-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. <span data-ttu-id="8ef00-155">Na hello **Panorama9 domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8ef00-155">On hello **Panorama9 Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    <span data-ttu-id="8ef00-157">a.</span><span class="sxs-lookup"><span data-stu-id="8ef00-157">a.</span></span> <span data-ttu-id="8ef00-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://dashboard.panorama9.com/saml/access/3262`</span><span class="sxs-lookup"><span data-stu-id="8ef00-158">In hello **Sign-on URL** textbox, type a URL as: `https://dashboard.panorama9.com/saml/access/3262`</span></span>

    <span data-ttu-id="8ef00-159">b.</span><span class="sxs-lookup"><span data-stu-id="8ef00-159">b.</span></span> <span data-ttu-id="8ef00-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`http://www.panorama9.com/saml20/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="8ef00-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://www.panorama9.com/saml20/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8ef00-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8ef00-161">These values are not real.</span></span> <span data-ttu-id="8ef00-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="8ef00-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8ef00-163">Obraťte se na [tým podpory Panorama9 klienta](https://support.panorama9.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8ef00-163">Contact [Panorama9 Client support team](https://support.panorama9.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="8ef00-164">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8ef00-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. <span data-ttu-id="8ef00-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ef00-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8ef00-168">Na hello **Panorama9 konfigurace** klikněte na tlačítko **konfigurace Panorama9** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8ef00-168">On hello **Panorama9 Configuration** section, click **Configure Panorama9** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8ef00-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8ef00-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. <span data-ttu-id="8ef00-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Panorama9.</span><span class="sxs-lookup"><span data-stu-id="8ef00-171">In a different web browser window, log into your Panorama9 company site as an administrator.</span></span>

6. <span data-ttu-id="8ef00-172">V panelu nástrojů hello hello nahoře, klikněte na **spravovat**a potom klikněte na **rozšíření**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-172">In hello toolbar on hello top, click **Manage**, and then click **Extensions**.</span></span>
   
   <span data-ttu-id="8ef00-173">![Rozšíření](./media/active-directory-saas-panorama9-tutorial/ic790023.png "rozšíření")</span><span class="sxs-lookup"><span data-stu-id="8ef00-173">![Extensions](./media/active-directory-saas-panorama9-tutorial/ic790023.png "Extensions")</span></span>
7. <span data-ttu-id="8ef00-174">Na hello **rozšíření** dialogové okno, klikněte na tlačítko **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-174">On hello **Extensions** dialog, click **Single Sign-On**.</span></span>
   
   <span data-ttu-id="8ef00-175">![Jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/ic790024.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="8ef00-175">![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/ic790024.png "Single Sign-On")</span></span>
8. <span data-ttu-id="8ef00-176">V hello **nastavení** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8ef00-176">In hello **Settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="8ef00-177">![Nastavení](./media/active-directory-saas-panorama9-tutorial/ic790025.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="8ef00-177">![Settings](./media/active-directory-saas-panorama9-tutorial/ic790025.png "Settings")</span></span>
   
    <span data-ttu-id="8ef00-178">a.</span><span class="sxs-lookup"><span data-stu-id="8ef00-178">a.</span></span> <span data-ttu-id="8ef00-179">V **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu hello **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ef00-179">In **Identity provider URL** textbox, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="8ef00-180">b.</span><span class="sxs-lookup"><span data-stu-id="8ef00-180">b.</span></span> <span data-ttu-id="8ef00-181">V **otisků prstů certifikátů** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ef00-181">In **Certificate fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>    
         
9. <span data-ttu-id="8ef00-182">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-182">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="8ef00-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="8ef00-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8ef00-184">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="8ef00-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8ef00-185">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8ef00-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ef00-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ef00-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ef00-187">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8ef00-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8ef00-189">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ef00-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ef00-190">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8ef00-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ef00-192">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ef00-194">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="8ef00-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ef00-196">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8ef00-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ef00-198">a.</span><span class="sxs-lookup"><span data-stu-id="8ef00-198">a.</span></span> <span data-ttu-id="8ef00-199">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ef00-200">b.</span><span class="sxs-lookup"><span data-stu-id="8ef00-200">b.</span></span> <span data-ttu-id="8ef00-201">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ef00-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ef00-202">c.</span><span class="sxs-lookup"><span data-stu-id="8ef00-202">c.</span></span> <span data-ttu-id="8ef00-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8ef00-204">d.</span><span class="sxs-lookup"><span data-stu-id="8ef00-204">d.</span></span> <span data-ttu-id="8ef00-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-205">Click **Create**.</span></span>
 
### <a name="creating-a-panorama9-test-user"></a><span data-ttu-id="8ef00-206">Vytvoření zkušebního uživatele Panorama9</span><span class="sxs-lookup"><span data-stu-id="8ef00-206">Creating a Panorama9 test user</span></span>

<span data-ttu-id="8ef00-207">V pořadí tooenable Azure AD Uživatelé toolog do Panorama9 musí být zřízená do Panorama9.</span><span class="sxs-lookup"><span data-stu-id="8ef00-207">In order tooenable Azure AD users toolog into Panorama9, they must be provisioned into Panorama9.</span></span>  

<span data-ttu-id="8ef00-208">V případě hello Panorama9 zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="8ef00-208">In hello case of Panorama9, provisioning is a manual task.</span></span>

<span data-ttu-id="8ef00-209">**tooconfigure zřizování uživatelů, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ef00-209">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ef00-210">Přihlaste se tooyour **Panorama9** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="8ef00-210">Log in tooyour **Panorama9** company site as an administrator.</span></span>

2. <span data-ttu-id="8ef00-211">V nabídce hello hello nahoře, klikněte na tlačítko **spravovat**a potom klikněte na **uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-211">In hello menu on hello top, click **Manage**, and then click **Users**.</span></span>
   
  <span data-ttu-id="8ef00-212">![Uživatelé](./media/active-directory-saas-panorama9-tutorial/ic790027.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="8ef00-212">![Users](./media/active-directory-saas-panorama9-tutorial/ic790027.png "Users")</span></span>

3. <span data-ttu-id="8ef00-213">V hello část Uživatelé, klikněte na  **+**  tooadd nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="8ef00-213">In hello Users section, Click **+** tooadd new user.</span></span>

 <span data-ttu-id="8ef00-214">![Uživatelé](./media/active-directory-saas-panorama9-tutorial/ic790028.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="8ef00-214">![Users](./media/active-directory-saas-panorama9-tutorial/ic790028.png "Users")</span></span>

4. <span data-ttu-id="8ef00-215">Přejděte toohello část dat uživatele, typu hello e-mailová adresa platného uživatele Azure Active Directory chcete tooprovision do hello **e-mailu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="8ef00-215">Go toohello User data section, type hello email address of a valid Azure Active Directory user you want tooprovision into hello **Email** textbox.</span></span>

5. <span data-ttu-id="8ef00-216">Pocházet toohello část Uživatelé, klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-216">Come toohello Users section, Click **Save**.</span></span>
   
> [!NOTE]
    > <span data-ttu-id="8ef00-217">Držitel účtu Azure Active Directory Hello obdrží e-mailu a dodržuje odkaz tooconfirm svého účtu před stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="8ef00-217">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8ef00-218">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8ef00-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8ef00-219">V této části povolíte tak, že udělíte přístup tooPanorama9 toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8ef00-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPanorama9.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8ef00-221">**tooassign Britta Simon tooPanorama9, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8ef00-221">**tooassign Britta Simon tooPanorama9, perform hello following steps:**</span></span>

1. <span data-ttu-id="8ef00-222">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8ef00-224">V seznamu aplikace hello vyberte **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-224">In hello applications list, select **Panorama9**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. <span data-ttu-id="8ef00-226">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8ef00-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8ef00-228">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8ef00-228">Click **Add** button.</span></span> <span data-ttu-id="8ef00-229">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ef00-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8ef00-231">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8ef00-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8ef00-232">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ef00-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ef00-233">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8ef00-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8ef00-234">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8ef00-234">Testing single sign-on</span></span>

<span data-ttu-id="8ef00-235">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8ef00-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8ef00-236">Když kliknete na dlaždici hello Panorama9 v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooPanorama9 aplikace.</span><span class="sxs-lookup"><span data-stu-id="8ef00-236">When you click hello Panorama9 tile in hello Access Panel, you should get automatically signed-on tooPanorama9 application.</span></span>
<span data-ttu-id="8ef00-237">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8ef00-237">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ef00-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8ef00-238">Additional resources</span></span>

* [<span data-ttu-id="8ef00-239">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8ef00-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ef00-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ef00-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

