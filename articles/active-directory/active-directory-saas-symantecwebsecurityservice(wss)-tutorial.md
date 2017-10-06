---
title: "Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS) | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Symantec webové zabezpečení služby (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: f4575e6f5eb63cd0e1d63edd35e30be3cacc3382
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="73f00-103">Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="73f00-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="73f00-104">V tomto kurzu zjistíte, jak toointegrate Symantec webové zabezpečení služby (WSS) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="73f00-104">In this tutorial, you learn how toointegrate Symantec Web Security Service (WSS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="73f00-105">Integrace Symantec webové zabezpečení služby (WSS) s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="73f00-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="73f00-106">Můžete řídit ve službě Azure AD, který má přístup tooSymantec zabezpečení webové služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="73f00-106">You can control in Azure AD who has access tooSymantec Web Security Service (WSS)</span></span>
- <span data-ttu-id="73f00-107">Vaši uživatelé tooautomatically get přihlášeného tooSymantec webové zabezpečení služby (WSS) (jednotné přihlášení) můžete povolit pomocí jejich účtů Azure AD</span><span class="sxs-lookup"><span data-stu-id="73f00-107">You can enable your users tooautomatically get signed-on tooSymantec Web Security Service (WSS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="73f00-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="73f00-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="73f00-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="73f00-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73f00-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="73f00-110">Prerequisites</span></span>

<span data-ttu-id="73f00-111">tooconfigure integrace Azure AD s Symantec webové zabezpečení služby (WSS), je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="73f00-111">tooconfigure Azure AD integration with Symantec Web Security Service (WSS), you need hello following items:</span></span>

- <span data-ttu-id="73f00-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="73f00-112">An Azure AD subscription</span></span>
- <span data-ttu-id="73f00-113">Symantec webové zabezpečení služby (WSS) jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="73f00-113">A Symantec Web Security Service (WSS) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="73f00-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="73f00-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="73f00-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="73f00-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="73f00-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="73f00-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="73f00-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="73f00-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="73f00-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="73f00-118">Scenario description</span></span>
<span data-ttu-id="73f00-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="73f00-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="73f00-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="73f00-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="73f00-121">Přidání Symantec webové zabezpečení služby (WSS) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="73f00-121">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
2. <span data-ttu-id="73f00-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="73f00-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a><span data-ttu-id="73f00-123">Přidání Symantec webové zabezpečení služby (WSS) z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="73f00-123">Adding Symantec Web Security Service (WSS) from hello gallery</span></span>
<span data-ttu-id="73f00-124">tooconfigure hello integrace Symantec webové zabezpečení služby (WSS) do Azure AD, je nutné tooadd Symantec webové zabezpečení služby (WSS) hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="73f00-124">tooconfigure hello integration of Symantec Web Security Service (WSS) into Azure AD, you need tooadd Symantec Web Security Service (WSS) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="73f00-125">**tooadd Symantec webové zabezpečení služby (WSS) z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="73f00-125">**tooadd Symantec Web Security Service (WSS) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="73f00-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="73f00-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="73f00-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="73f00-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="73f00-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="73f00-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="73f00-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="73f00-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="73f00-133">Hello vyhledávacího pole zadejte **Symantec webové zabezpečení služby (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="73f00-133">In hello search box, type **Symantec Web Security Service (WSS)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. <span data-ttu-id="73f00-135">Na panelu výsledků hello vyberte **Symantec webové zabezpečení služby (WSS)**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="73f00-135">In hello results panel, select **Symantec Web Security Service (WSS)**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="73f00-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="73f00-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="73f00-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="73f00-138">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="73f00-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Symantec webové zabezpečení služby (WSS) je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="73f00-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Symantec Web Security Service (WSS) is tooa user in Azure AD.</span></span> <span data-ttu-id="73f00-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Symantec webové zabezpečení služby (WSS) musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="73f00-140">In other words, a link relationship between an Azure AD user and hello related user in Symantec Web Security Service (WSS) needs toobe established.</span></span>

<span data-ttu-id="73f00-141">V Symantec webové zabezpečení služby (WSS) přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="73f00-141">In Symantec Web Security Service (WSS), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="73f00-142">tooconfigure a testování Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), je třeba toocomplete hello stavební bloky následující:</span><span class="sxs-lookup"><span data-stu-id="73f00-142">tooconfigure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="73f00-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="73f00-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="73f00-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="73f00-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="73f00-145">**[Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)](#creating-a-symantec-web-security-service-wss-test-user)**  -toohave protějšek Britta Simon v zabezpečení webové Symantec služby (WSS), je toohello propojené služby Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="73f00-145">**[Creating a Symantec Web Security Service (WSS) test user](#creating-a-symantec-web-security-service-wss-test-user)** - toohave a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="73f00-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="73f00-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="73f00-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="73f00-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="73f00-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="73f00-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="73f00-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="73f00-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="73f00-150">**tooconfigure Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="73f00-150">**tooconfigure Azure AD single sign-on with Symantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="73f00-151">V portálu Azure, na hello hello **Symantec webové zabezpečení služby (WSS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="73f00-151">In hello Azure portal, on hello **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="73f00-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="73f00-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="73f00-155">Na hello **Symantec webové zabezpečení služby (WSS) domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="73f00-155">On hello **Symantec Web Security Service (WSS) Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="73f00-157">a.</span><span class="sxs-lookup"><span data-stu-id="73f00-157">a.</span></span> <span data-ttu-id="73f00-158">V hello **přihlašovací adresa URL** textovému poli, zmínky hello url, která chcete tooblock podle zásady pro blokování toohello Dobrý den Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="73f00-158">In hello **Sign-on URL** textbox, mention hello url, which you want tooblock according toohello blocking policy of hello Symantec Web Security Service (WSS).</span></span>  
    
    <span data-ttu-id="73f00-159">b.</span><span class="sxs-lookup"><span data-stu-id="73f00-159">b.</span></span> <span data-ttu-id="73f00-160">V hello **identifikátor** textovému poli, hello zadat adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="73f00-160">In hello **Identifier** textbox, type hello URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="73f00-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="73f00-161">These values are not real.</span></span> <span data-ttu-id="73f00-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="73f00-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="73f00-163">Obraťte se na [tým podpory Symantec webové zabezpečení služby (WSS) klienta](https://www.symantec.com/contact-us) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="73f00-163">Contact [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) tooget these values.</span></span> 

4. <span data-ttu-id="73f00-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="73f00-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="73f00-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="73f00-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="73f00-168">tooconfigure jednotného přihlašování na **Symantec webové zabezpečení služby (WSS)** straně, je nutné stáhnout hello toosend **soubor XML s metadaty** příliš[Symantec webové zabezpečení služby (WSS) podporují team](https://www.symantec.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="73f00-168">tooconfigure single sign-on on **Symantec Web Security Service (WSS)** side, you need toosend hello downloaded **Metadata XML** too[Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="73f00-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="73f00-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="73f00-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="73f00-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="73f00-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="73f00-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="73f00-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="73f00-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="73f00-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="73f00-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="73f00-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="73f00-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="73f00-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="73f00-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="73f00-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="73f00-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="73f00-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="73f00-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="73f00-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="73f00-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="73f00-184">a.</span><span class="sxs-lookup"><span data-stu-id="73f00-184">a.</span></span> <span data-ttu-id="73f00-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="73f00-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="73f00-186">b.</span><span class="sxs-lookup"><span data-stu-id="73f00-186">b.</span></span> <span data-ttu-id="73f00-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="73f00-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="73f00-188">c.</span><span class="sxs-lookup"><span data-stu-id="73f00-188">c.</span></span> <span data-ttu-id="73f00-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="73f00-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="73f00-190">d.</span><span class="sxs-lookup"><span data-stu-id="73f00-190">d.</span></span> <span data-ttu-id="73f00-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="73f00-191">Click **Create**.</span></span>
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="73f00-192">Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="73f00-192">Creating a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="73f00-193">V této části vytvoříte uživatele volat Britta Simon v Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="73f00-193">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="73f00-194">Práce s [tým podpory Symantec webové zabezpečení služby (WSS)](https://www.symantec.com/contact-us) pro přidání uživatelů hello platformy hello Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="73f00-194">Work with [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) to add hello users in hello Symantec Web Security Service (WSS) platform.</span></span> <span data-ttu-id="73f00-195">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="73f00-195">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="73f00-196">Také společně s hello uživatele podrobnosti budete potřebovat toosend hello veřejná IP adresa hello počítače, ze kterého se pokoušíte tooaccess hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="73f00-196">Also along with hello user details you need toosend hello public IPaddress of hello machine from which you are trying tooaccess hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="73f00-197">Prosím [kliknutím sem](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget váš počítač je veřejná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="73f00-197">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget your machine's public IPaddress.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="73f00-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="73f00-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="73f00-199">V této části povolíte tak, že udělíte přístup tooSymantec zabezpečení webové služby (WSS) toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="73f00-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSymantec Web Security Service (WSS).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="73f00-201">**tooassign tooSymantec Britta Simon webové zabezpečení služby (WSS), proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="73f00-201">**tooassign Britta Simon tooSymantec Web Security Service (WSS), perform hello following steps:**</span></span>

1. <span data-ttu-id="73f00-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="73f00-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="73f00-204">V seznamu aplikace hello vyberte **Symantec webové zabezpečení služby (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="73f00-204">In hello applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. <span data-ttu-id="73f00-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="73f00-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="73f00-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="73f00-208">Click **Add** button.</span></span> <span data-ttu-id="73f00-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73f00-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="73f00-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="73f00-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="73f00-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73f00-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="73f00-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="73f00-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="73f00-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="73f00-214">Testing single sign-on</span></span>

<span data-ttu-id="73f00-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="73f00-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="73f00-216">Po kliknutí na tlačítko hello Symantec webové zabezpečení služby (WSS) dlaždici v hello přístupového panelu, získáte přesměrovaného toohello blokování stránky, pro který byl nakonfigurován hello blokování zásad.</span><span class="sxs-lookup"><span data-stu-id="73f00-216">When you click hello Symantec Web Security Service (WSS) tile in hello Access Panel, you get redirected toohello blocking page for which hello blocking policy has been configured.</span></span>
<span data-ttu-id="73f00-217">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="73f00-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73f00-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="73f00-218">Additional resources</span></span>

* [<span data-ttu-id="73f00-219">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="73f00-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="73f00-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="73f00-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

