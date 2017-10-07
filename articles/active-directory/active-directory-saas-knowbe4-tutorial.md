---
title: "Kurz: Azure Active Directory integrace s KnowBe4 zabezpečení sledování školení | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a KnowBe4 zabezpečení sledování školení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 907fa814b82c9ffb2376f73470b746a37104c66e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a><span data-ttu-id="b4efa-103">Kurz: Azure Active Directory integrace s KnowBe4 zabezpečení sledování školení</span><span class="sxs-lookup"><span data-stu-id="b4efa-103">Tutorial: Azure Active Directory integration with KnowBe4 Security Awareness Training</span></span>

<span data-ttu-id="b4efa-104">V tomto kurzu zjistíte, jak toointegrate školení sledování KnowBe4 zabezpečení v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b4efa-104">In this tutorial, you learn how toointegrate KnowBe4 Security Awareness Training with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b4efa-105">Integrace KnowBe4 zabezpečení sledování školení s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="b4efa-105">Integrating KnowBe4 Security Awareness Training with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b4efa-106">Můžete řídit v Azure AD, který má přístup k tooKnowBe4 zabezpečení sledování školení</span><span class="sxs-lookup"><span data-stu-id="b4efa-106">You can control in Azure AD who has access tooKnowBe4 Security Awareness Training</span></span>
- <span data-ttu-id="b4efa-107">Můžete povolit uživatelům získat tooautomatically přihlášeného tooKnowBe4 bezpečnostního školení sledování (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4efa-107">You can enable your users tooautomatically get signed-on tooKnowBe4 Security Awareness Training (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b4efa-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b4efa-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b4efa-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b4efa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4efa-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b4efa-110">Prerequisites</span></span>

<span data-ttu-id="b4efa-111">tooconfigure integrace Azure AD se školením sledování zabezpečení KnowBe4, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="b4efa-111">tooconfigure Azure AD integration with KnowBe4 Security Awareness Training, you need hello following items:</span></span>

- <span data-ttu-id="b4efa-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4efa-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b4efa-113">Školení sledování zabezpečení KnowBe4 jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="b4efa-113">A KnowBe4 Security Awareness Training single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b4efa-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4efa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b4efa-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="b4efa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b4efa-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="b4efa-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b4efa-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4efa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b4efa-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="b4efa-118">Scenario description</span></span>
<span data-ttu-id="b4efa-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4efa-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b4efa-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="b4efa-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b4efa-121">Přidání KnowBe4 zabezpečení sledování školení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b4efa-121">Adding KnowBe4 Security Awareness Training from hello gallery</span></span>
2. <span data-ttu-id="b4efa-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4efa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-knowbe4-security-awareness-training-from-hello-gallery"></a><span data-ttu-id="b4efa-123">Přidání KnowBe4 zabezpečení sledování školení z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="b4efa-123">Adding KnowBe4 Security Awareness Training from hello gallery</span></span>
<span data-ttu-id="b4efa-124">tooconfigure hello integrace KnowBe4 zabezpečení sledování školení do Azure AD, je nutné tooadd KnowBe4 zabezpečení sledování školení hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="b4efa-124">tooconfigure hello integration of KnowBe4 Security Awareness Training into Azure AD, you need tooadd KnowBe4 Security Awareness Training from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b4efa-125">**tooadd KnowBe4 bezpečnostního školení sledování z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4efa-125">**tooadd KnowBe4 Security Awareness Training from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4efa-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b4efa-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b4efa-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b4efa-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="b4efa-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4efa-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="b4efa-133">Hello vyhledávacího pole zadejte **KnowBe4 zabezpečení sledování školení**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-133">In hello search box, type **KnowBe4 Security Awareness Training**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_search.png)

5. <span data-ttu-id="b4efa-135">Na panelu výsledků hello vyberte **KnowBe4 zabezpečení sledování školení**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4efa-135">In hello results panel, select **KnowBe4 Security Awareness Training**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b4efa-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4efa-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b4efa-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s KnowBe4 zabezpečení sledování školení podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="b4efa-138">In this section, you configure and test Azure AD single sign-on with KnowBe4 Security Awareness Training based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b4efa-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v KnowBe4 zabezpečení sledování školení je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b4efa-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in KnowBe4 Security Awareness Training is tooa user in Azure AD.</span></span> <span data-ttu-id="b4efa-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v KnowBe4 zabezpečení sledování školení musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="b4efa-140">In other words, a link relationship between an Azure AD user and hello related user in KnowBe4 Security Awareness Training needs toobe established.</span></span>

<span data-ttu-id="b4efa-141">V KnowBe4 zabezpečení sledování školení, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="b4efa-141">In KnowBe4 Security Awareness Training, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b4efa-142">tooconfigure a testování Azure AD jednotné přihlašování se školením sledování KnowBe4 zabezpečení, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="b4efa-142">tooconfigure and test Azure AD single sign-on with KnowBe4 Security Awareness Training, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b4efa-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="b4efa-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b4efa-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4efa-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b4efa-145">**[Vytvoření zkušebního uživatele školení sledování zabezpečení KnowBe4](#creating-a-knowbe4-security-awareness-training-test-user)**  -toohave protějšek Britta Simon v KnowBe4 školení sledování zabezpečení, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4efa-145">**[Creating a KnowBe4 Security Awareness Training test user](#creating-a-knowbe4-security-awareness-training-test-user)** - toohave a counterpart of Britta Simon in KnowBe4 Security Awareness Training that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b4efa-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4efa-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b4efa-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="b4efa-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b4efa-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4efa-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b4efa-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci KnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="b4efa-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your KnowBe4 Security Awareness Training application.</span></span>

<span data-ttu-id="b4efa-150">**tooconfigure Azure AD jednotné přihlašování se školením sledování zabezpečení KnowBe4, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4efa-150">**tooconfigure Azure AD single sign-on with KnowBe4 Security Awareness Training, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4efa-151">V portálu Azure, na hello hello **KnowBe4 zabezpečení sledování školení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-151">In hello Azure portal, on hello **KnowBe4 Security Awareness Training** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="b4efa-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4efa-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_samlbase.png)

3. <span data-ttu-id="b4efa-155">Na hello **KnowBe4 zabezpečení sledování školení domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="b4efa-155">On hello **KnowBe4 Security Awareness Training Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_url.png)

    <span data-ttu-id="b4efa-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<companyname>.KnowBe4.com/auth/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="b4efa-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b4efa-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="b4efa-158">hello value is not real.</span></span> <span data-ttu-id="b4efa-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="b4efa-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b4efa-160">Obraťte se na [tým podpory klienta školení sledování zabezpečení KnowBe4](mailto:support@KnowBe4.com) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b4efa-160">Contact [KnowBe4 Security Awareness Training Client support team](mailto:support@KnowBe4.com) tooget hello value.</span></span> 
 

4. <span data-ttu-id="b4efa-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="b4efa-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_certificate.png) 

5. <span data-ttu-id="b4efa-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4efa-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b4efa-165">Na hello **konfigurace školení sledování zabezpečení KnowBe4** klikněte na tlačítko **konfigurace KnowBe4 zabezpečení sledování školení** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="b4efa-165">On hello **KnowBe4 Security Awareness Training Configuration** section, click **Configure KnowBe4 Security Awareness Training** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b4efa-166">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="b4efa-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_configure.png) 

7. <span data-ttu-id="b4efa-168">tooconfigure jednotného přihlašování na **KnowBe4 zabezpečení sledování školení** straně, je nutné stáhnout hello toosend **certifikátu (Raw)**, **Sign-Out adresu URL, SAML Entity ID a SAML jednotné přihlašování Adresa URL služby** příliš[tým podpory klienta školení sledování zabezpečení KnowBe4](mailto:support@KnowBe4.com).</span><span class="sxs-lookup"><span data-stu-id="b4efa-168">tooconfigure single sign-on on **KnowBe4 Security Awareness Training** side, you need toosend hello downloaded **Certificate (Raw)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[KnowBe4 Security Awareness Training Client support team](mailto:support@KnowBe4.com).</span></span>

> [!TIP]
> <span data-ttu-id="b4efa-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="b4efa-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b4efa-170">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="b4efa-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b4efa-171">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b4efa-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b4efa-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4efa-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="b4efa-173">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="b4efa-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="b4efa-175">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4efa-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4efa-176">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="b4efa-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b4efa-178">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b4efa-180">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="b4efa-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b4efa-182">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b4efa-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b4efa-184">a.</span><span class="sxs-lookup"><span data-stu-id="b4efa-184">a.</span></span> <span data-ttu-id="b4efa-185">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b4efa-186">b.</span><span class="sxs-lookup"><span data-stu-id="b4efa-186">b.</span></span> <span data-ttu-id="b4efa-187">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b4efa-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b4efa-188">c.</span><span class="sxs-lookup"><span data-stu-id="b4efa-188">c.</span></span> <span data-ttu-id="b4efa-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b4efa-190">d.</span><span class="sxs-lookup"><span data-stu-id="b4efa-190">d.</span></span> <span data-ttu-id="b4efa-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-191">Click **Create**.</span></span>
 
### <a name="creating-a-knowbe4-security-awareness-training-test-user"></a><span data-ttu-id="b4efa-192">Vytvoření zkušebního uživatele KnowBe4 zabezpečení sledování školení</span><span class="sxs-lookup"><span data-stu-id="b4efa-192">Creating a KnowBe4 Security Awareness Training test user</span></span>

<span data-ttu-id="b4efa-193">Hello cílem této části je toocreate volal Britta Simon v školení sledování KnowBe4 zabezpečení uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4efa-193">hello objective of this section is toocreate a user called Britta Simon in KnowBe4 Security Awareness Training.</span></span> <span data-ttu-id="b4efa-194">Školení sledování zabezpečení KnowBe4 podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="b4efa-194">KnowBe4 Security Awareness Training supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="b4efa-195">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="b4efa-195">There is no action item for you in this section.</span></span> <span data-ttu-id="b4efa-196">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess KnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="b4efa-196">A new user is created during an attempt tooaccess KnowBe4 Security Awareness Training if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="b4efa-197">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory školení sledování zabezpečení KnowBe4](mailto:support@KnowBe4.com).</span><span class="sxs-lookup"><span data-stu-id="b4efa-197">If you need toocreate a user manually, you need toocontact hello [KnowBe4 Security Awareness Training support team](mailto:support@KnowBe4.com).</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b4efa-198">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="b4efa-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b4efa-199">V této části povolíte Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooKnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="b4efa-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKnowBe4 Security Awareness Training.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="b4efa-201">**tooassign Britta Simon tooKnowBe4 zabezpečení sledování školení, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="b4efa-201">**tooassign Britta Simon tooKnowBe4 Security Awareness Training, perform hello following steps:**</span></span>

1. <span data-ttu-id="b4efa-202">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="b4efa-204">V seznamu aplikace hello vyberte **KnowBe4 zabezpečení sledování školení**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-204">In hello applications list, select **KnowBe4 Security Awareness Training**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_app.png) 

3. <span data-ttu-id="b4efa-206">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="b4efa-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="b4efa-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4efa-208">Click **Add** button.</span></span> <span data-ttu-id="b4efa-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4efa-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="b4efa-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="b4efa-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b4efa-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4efa-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b4efa-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4efa-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b4efa-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="b4efa-214">Testing single sign-on</span></span>

<span data-ttu-id="b4efa-215">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="b4efa-215">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>
  
<span data-ttu-id="b4efa-216">Když kliknete na dlaždici KnowBe4 zabezpečení sledování školení hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour KnowBe4 zabezpečení sledování školení aplikace.</span><span class="sxs-lookup"><span data-stu-id="b4efa-216">When you click hello KnowBe4 Security Awareness Training tile in hello Access Panel, you should get automatically signed-on tooyour KnowBe4 Security Awareness Training application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4efa-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="b4efa-217">Additional resources</span></span>

* [<span data-ttu-id="b4efa-218">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b4efa-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b4efa-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b4efa-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_203.png

