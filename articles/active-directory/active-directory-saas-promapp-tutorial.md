---
title: 'Kurz: Azure Active Directory integrace s Promapp | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Promapp."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 02de7679b0c86d7aa8cacb41762f900dbf2ff231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a><span data-ttu-id="6d18e-103">Kurz: Azure Active Directory integrace s Promapp</span><span class="sxs-lookup"><span data-stu-id="6d18e-103">Tutorial: Azure Active Directory integration with Promapp</span></span>

<span data-ttu-id="6d18e-104">V tomto kurzu zjistíte, jak toointegrate Promapp s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6d18e-104">In this tutorial, you learn how toointegrate Promapp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6d18e-105">Integrace Promapp s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6d18e-105">Integrating Promapp with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6d18e-106">Můžete řídit ve službě Azure AD, který má přístup tooPromapp</span><span class="sxs-lookup"><span data-stu-id="6d18e-106">You can control in Azure AD who has access tooPromapp</span></span>
- <span data-ttu-id="6d18e-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooPromapp (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d18e-107">You can enable your users tooautomatically get signed-on tooPromapp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6d18e-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6d18e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6d18e-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6d18e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6d18e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6d18e-110">Prerequisites</span></span>

<span data-ttu-id="6d18e-111">Integrace služby Azure AD s Promapp tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="6d18e-111">tooconfigure Azure AD integration with Promapp, you need hello following items:</span></span>

- <span data-ttu-id="6d18e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d18e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6d18e-113">Promapp jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6d18e-113">A Promapp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6d18e-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d18e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6d18e-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6d18e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6d18e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6d18e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6d18e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6d18e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6d18e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6d18e-118">Scenario description</span></span>
<span data-ttu-id="6d18e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6d18e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6d18e-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6d18e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6d18e-121">Přidání Promapp z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6d18e-121">Adding Promapp from hello gallery</span></span>
2. <span data-ttu-id="6d18e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d18e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-promapp-from-hello-gallery"></a><span data-ttu-id="6d18e-123">Přidání Promapp z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="6d18e-123">Adding Promapp from hello gallery</span></span>
<span data-ttu-id="6d18e-124">tooconfigure hello integrace Promapp do Azure AD, je nutné tooadd Promapp hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="6d18e-124">tooconfigure hello integration of Promapp into Azure AD, you need tooadd Promapp from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6d18e-125">**tooadd Promapp z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d18e-125">**tooadd Promapp from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d18e-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6d18e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6d18e-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6d18e-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6d18e-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d18e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6d18e-133">Hello vyhledávacího pole zadejte **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-133">In hello search box, type **Promapp**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_search.png)

5. <span data-ttu-id="6d18e-135">Na panelu výsledků hello vyberte **Promapp**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d18e-135">In hello results panel, select **Promapp**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6d18e-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d18e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6d18e-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Promapp podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="6d18e-138">In this section, you configure and test Azure AD single sign-on with Promapp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6d18e-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Promapp je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6d18e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Promapp is tooa user in Azure AD.</span></span> <span data-ttu-id="6d18e-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Promapp musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="6d18e-140">In other words, a link relationship between an Azure AD user and hello related user in Promapp needs toobe established.</span></span>

<span data-ttu-id="6d18e-141">V Promapp, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="6d18e-141">In Promapp, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6d18e-142">tooconfigure a testu Azure AD jednotné přihlašování s Promapp, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="6d18e-142">tooconfigure and test Azure AD single sign-on with Promapp, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6d18e-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="6d18e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6d18e-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6d18e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6d18e-145">**[Vytvoření zkušebního uživatele Promapp](#creating-a-promapp-test-user)**  -toohave protějšek Britta Simon v Promapp, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="6d18e-145">**[Creating a Promapp test user](#creating-a-promapp-test-user)** - toohave a counterpart of Britta Simon in Promapp that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6d18e-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d18e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6d18e-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="6d18e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6d18e-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d18e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6d18e-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Promapp.</span><span class="sxs-lookup"><span data-stu-id="6d18e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Promapp application.</span></span>

<span data-ttu-id="6d18e-150">**tooconfigure Azure AD jednotné přihlašování s Promapp, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d18e-150">**tooconfigure Azure AD single sign-on with Promapp, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d18e-151">V portálu Azure, na hello hello **Promapp** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-151">In hello Azure portal, on hello **Promapp** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6d18e-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d18e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_samlbase.png)

3. <span data-ttu-id="6d18e-155">Na hello **Promapp domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6d18e-155">On hello **Promapp Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url.png)

    <span data-ttu-id="6d18e-157">a.</span><span class="sxs-lookup"><span data-stu-id="6d18e-157">a.</span></span> <span data-ttu-id="6d18e-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span><span class="sxs-lookup"><span data-stu-id="6d18e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span></span>

    <span data-ttu-id="6d18e-159">b.</span><span class="sxs-lookup"><span data-stu-id="6d18e-159">b.</span></span> <span data-ttu-id="6d18e-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://DOMAINNAME.promapp.com/TENANTNAME`</span><span class="sxs-lookup"><span data-stu-id="6d18e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6d18e-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="6d18e-161">These values are not real.</span></span> <span data-ttu-id="6d18e-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="6d18e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6d18e-163">Obraťte se na [tým podpory Promapp klienta](https://www.promapp.com/about-us/contact-us/) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6d18e-163">Contact [Promapp Client support team](https://www.promapp.com/about-us/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="6d18e-164">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="6d18e-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_certificate.png) 

5. <span data-ttu-id="6d18e-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d18e-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6d18e-168">Na hello **Promapp konfigurace** klikněte na tlačítko **konfigurace Promapp** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6d18e-168">On hello **Promapp Configuration** section, click **Configure Promapp** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6d18e-169">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6d18e-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_configure.png) 

7. <span data-ttu-id="6d18e-171">Web společnosti Promapp tooyour přihlášení jako správce.</span><span class="sxs-lookup"><span data-stu-id="6d18e-171">Sign-on tooyour Promapp company site as administrator.</span></span> 

8. <span data-ttu-id="6d18e-172">V nabídce hello hello nahoře, klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-172">In hello menu on hello top, click **Admin**.</span></span> 
   
    ![Azure AD jednotné přihlášení][12]

9. <span data-ttu-id="6d18e-174">Klikněte na **Konfigurovat**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-174">Click **Configure**.</span></span> 
   
    ![Azure AD jednotné přihlášení][13]

10. <span data-ttu-id="6d18e-176">Na hello **zabezpečení** dialogové okno, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="6d18e-176">On hello **Security** dialog, perform hello following steps:</span></span>
   
    ![Azure AD jednotné přihlášení][14]
    
    <span data-ttu-id="6d18e-178">a.</span><span class="sxs-lookup"><span data-stu-id="6d18e-178">a.</span></span> <span data-ttu-id="6d18e-179">Vložení **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z hello portálu Azure do hello **adresu URL pro přihlášení SSO** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6d18e-179">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **SSO-Login URL** textbox.</span></span>
    
    <span data-ttu-id="6d18e-180">b.</span><span class="sxs-lookup"><span data-stu-id="6d18e-180">b.</span></span> <span data-ttu-id="6d18e-181">Jako **jednotné přihlašování – jednotné přihlašování v režimu**, vyberte **volitelné**a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-181">As **SSO - Single Sign-on Mode**, select **Optional**, and then click **Save**.</span></span>

    <span data-ttu-id="6d18e-182">c.</span><span class="sxs-lookup"><span data-stu-id="6d18e-182">c.</span></span> <span data-ttu-id="6d18e-183">Otevřete hello stáhnout certifikát v poznámkovém bloku kopírování obsahu certifikátu hello bez hello prvního řádku (---BEGIN CERTIFICATE---) a hello poslední řádek (---END CERTIFICATE---), vložte jej do hello **certifikát x.509 jednotného přihlašování k** textové pole a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-183">Open hello downloaded certificate in notepad, copy hello certificate content without hello first line (-----BEGIN CERTIFICATE-----) and hello last line (-----END CERTIFICATE-----), paste it into hello **SSO-x.509 Certificate** textbox, and then click **Save**.</span></span>
        
> [!TIP]
> <span data-ttu-id="6d18e-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="6d18e-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6d18e-185">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="6d18e-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6d18e-186">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6d18e-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6d18e-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6d18e-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="6d18e-188">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="6d18e-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6d18e-190">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d18e-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d18e-191">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6d18e-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6d18e-193">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6d18e-195">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="6d18e-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6d18e-197">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6d18e-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-promapp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6d18e-199">a.</span><span class="sxs-lookup"><span data-stu-id="6d18e-199">a.</span></span> <span data-ttu-id="6d18e-200">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6d18e-201">b.</span><span class="sxs-lookup"><span data-stu-id="6d18e-201">b.</span></span> <span data-ttu-id="6d18e-202">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6d18e-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6d18e-203">c.</span><span class="sxs-lookup"><span data-stu-id="6d18e-203">c.</span></span> <span data-ttu-id="6d18e-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6d18e-205">d.</span><span class="sxs-lookup"><span data-stu-id="6d18e-205">d.</span></span> <span data-ttu-id="6d18e-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-206">Click **Create**.</span></span>
 
### <a name="creating-a-promapp-test-user"></a><span data-ttu-id="6d18e-207">Vytvoření zkušebního uživatele Promapp</span><span class="sxs-lookup"><span data-stu-id="6d18e-207">Creating a Promapp test user</span></span>

<span data-ttu-id="6d18e-208">Hello Promapp aplikace podporuje pouze za běhu zřizování.</span><span class="sxs-lookup"><span data-stu-id="6d18e-208">hello Promapp application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="6d18e-209">To znamená, uživatelský účet se automaticky vytvoří v případě potřeby během pokusu o aplikace hello tooaccess pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6d18e-209">This means, a user account is automatically created if necessary during an attempt tooaccess hello application using hello Access Panel.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6d18e-210">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="6d18e-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6d18e-211">V této části povolíte tak, že udělíte přístup tooPromapp toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6d18e-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPromapp.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6d18e-213">**tooassign Britta Simon tooPromapp, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="6d18e-213">**tooassign Britta Simon tooPromapp, perform hello following steps:**</span></span>

1. <span data-ttu-id="6d18e-214">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6d18e-216">V seznamu aplikace hello vyberte **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-216">In hello applications list, select **Promapp**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_app.png) 

3. <span data-ttu-id="6d18e-218">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6d18e-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6d18e-220">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6d18e-220">Click **Add** button.</span></span> <span data-ttu-id="6d18e-221">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d18e-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6d18e-223">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="6d18e-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6d18e-224">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d18e-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6d18e-225">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6d18e-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6d18e-226">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6d18e-226">Testing single sign-on</span></span>

<span data-ttu-id="6d18e-227">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="6d18e-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="6d18e-228">Když kliknete na dlaždici Promapp hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Promapp aplikace.</span><span class="sxs-lookup"><span data-stu-id="6d18e-228">When you click hello Promapp tile in hello Access Panel, you should get automatically signed-on tooyour Promapp application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d18e-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6d18e-229">Additional resources</span></span>

* [<span data-ttu-id="6d18e-230">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d18e-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6d18e-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6d18e-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_203.png

