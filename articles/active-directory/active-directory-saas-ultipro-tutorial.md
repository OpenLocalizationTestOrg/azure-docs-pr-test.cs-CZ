---
title: 'Kurz: Azure Active Directory integrace s UltiPro | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a UltiPro."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: afc0f2b9-2eac-47ec-af04-65ed0fb0ca5a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 8cb613228893e8cbf997957452d55d0ab7939171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ultipro"></a><span data-ttu-id="beec9-103">Kurz: Azure Active Directory integrace s UltiPro</span><span class="sxs-lookup"><span data-stu-id="beec9-103">Tutorial: Azure Active Directory integration with UltiPro</span></span>

<span data-ttu-id="beec9-104">V tomto kurzu zjistíte, jak toointegrate UltiPro s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="beec9-104">In this tutorial, you learn how toointegrate UltiPro with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="beec9-105">Integrace UltiPro s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="beec9-105">Integrating UltiPro with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="beec9-106">Můžete řídit ve službě Azure AD, který má přístup tooUltiPro</span><span class="sxs-lookup"><span data-stu-id="beec9-106">You can control in Azure AD who has access tooUltiPro</span></span>
- <span data-ttu-id="beec9-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooUltiPro (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="beec9-107">You can enable your users tooautomatically get signed-on tooUltiPro (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="beec9-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="beec9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="beec9-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="beec9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="beec9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="beec9-110">Prerequisites</span></span>

<span data-ttu-id="beec9-111">Integrace služby Azure AD s UltiPro tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="beec9-111">tooconfigure Azure AD integration with UltiPro, you need hello following items:</span></span>

- <span data-ttu-id="beec9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="beec9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="beec9-113">UltiPro jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="beec9-113">A UltiPro single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="beec9-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="beec9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="beec9-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="beec9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="beec9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="beec9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="beec9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="beec9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="beec9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="beec9-118">Scenario description</span></span>
<span data-ttu-id="beec9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="beec9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="beec9-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="beec9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="beec9-121">Přidání UltiPro z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="beec9-121">Adding UltiPro from hello gallery</span></span>
2. <span data-ttu-id="beec9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="beec9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ultipro-from-hello-gallery"></a><span data-ttu-id="beec9-123">Přidání UltiPro z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="beec9-123">Adding UltiPro from hello gallery</span></span>
<span data-ttu-id="beec9-124">tooconfigure hello integrace UltiPro do Azure AD, je nutné tooadd UltiPro hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="beec9-124">tooconfigure hello integration of UltiPro into Azure AD, you need tooadd UltiPro from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="beec9-125">**tooadd UltiPro z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="beec9-125">**tooadd UltiPro from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="beec9-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="beec9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="beec9-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="beec9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="beec9-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="beec9-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="beec9-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="beec9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="beec9-133">Hello vyhledávacího pole zadejte **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="beec9-133">In hello search box, type **UltiPro**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_search.png)

5. <span data-ttu-id="beec9-135">Na panelu výsledků hello vyberte **UltiPro**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="beec9-135">In hello results panel, select **UltiPro**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="beec9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="beec9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="beec9-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s UltiPro podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="beec9-138">In this section, you configure and test Azure AD single sign-on with UltiPro based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="beec9-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v UltiPro je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="beec9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UltiPro is tooa user in Azure AD.</span></span> <span data-ttu-id="beec9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v UltiPro musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="beec9-140">In other words, a link relationship between an Azure AD user and hello related user in UltiPro needs toobe established.</span></span>

<span data-ttu-id="beec9-141">V UltiPro, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="beec9-141">In UltiPro, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="beec9-142">tooconfigure a testu Azure AD jednotné přihlašování s UltiPro, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="beec9-142">tooconfigure and test Azure AD single sign-on with UltiPro, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="beec9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="beec9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="beec9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="beec9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="beec9-145">**[Vytvoření zkušebního uživatele UltiPro](#creating-a-ultipro-test-user)**  -toohave protějšek Britta Simon v UltiPro, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="beec9-145">**[Creating a UltiPro test user](#creating-a-ultipro-test-user)** - toohave a counterpart of Britta Simon in UltiPro that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="beec9-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="beec9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="beec9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="beec9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="beec9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="beec9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="beec9-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci UltiPro.</span><span class="sxs-lookup"><span data-stu-id="beec9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UltiPro application.</span></span>

<span data-ttu-id="beec9-150">**tooconfigure Azure AD jednotné přihlašování s UltiPro, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="beec9-150">**tooconfigure Azure AD single sign-on with UltiPro, perform hello following steps:**</span></span>

1. <span data-ttu-id="beec9-151">V portálu Azure, na hello hello **UltiPro** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="beec9-151">In hello Azure portal, on hello **UltiPro** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="beec9-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="beec9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_samlbase.png)

3. <span data-ttu-id="beec9-155">Na hello **UltiPro domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="beec9-155">On hello **UltiPro Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_url.png)

    <span data-ttu-id="beec9-157">a.</span><span class="sxs-lookup"><span data-stu-id="beec9-157">a.</span></span> <span data-ttu-id="beec9-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="beec9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/`|
    | `https://<companyname>.ultiproworkplace.com?cpi=AZUREADISSSUERURL`|
    | ` https://<companyname>.ultipro.ca`|
    
    <span data-ttu-id="beec9-159">b.</span><span class="sxs-lookup"><span data-stu-id="beec9-159">b.</span></span> <span data-ttu-id="beec9-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="beec9-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/adfs/services/trust`|
    | `https://<companyname>.ultiproworkplace.com/adfs/services/trust`|
    | `https://<companyname>.ultipro.ca/adfs/services/trust`|
    
    <span data-ttu-id="beec9-161">c.</span><span class="sxs-lookup"><span data-stu-id="beec9-161">c.</span></span> <span data-ttu-id="beec9-162">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="beec9-162">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.ultipro.com/<instancename>`|
    | `https://<companyname>.ultiproworkplace.com/<instancename>`|
    | `https://<companyname>.ultipro.ca/<instancename>`|
    
    > [!NOTE] 
    > <span data-ttu-id="beec9-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="beec9-163">These values are not real.</span></span> <span data-ttu-id="beec9-164">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="beec9-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="beec9-165">Obraťte se na [tým podpory UltiPro klienta](https://www.ultimatesoftware.com/ContactUs) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="beec9-165">Contact [UltiPro Client support team](https://www.ultimatesoftware.com/ContactUs) tooget these values.</span></span> 

5. <span data-ttu-id="beec9-166">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="beec9-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_certificate.png) 

6. <span data-ttu-id="beec9-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="beec9-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="beec9-170">Na hello **UltiPro konfigurace** klikněte na tlačítko **konfigurace UltiPro** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="beec9-170">On hello **UltiPro Configuration** section, click **Configure UltiPro** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="beec9-171">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="beec9-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_configure.png) 

8. <span data-ttu-id="beec9-173">tooconfigure jednotného přihlašování na **UltiPro** straně, je nutné stáhnout hello toosend **Certificate(Base64), adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[UltiPro tým podpory](https://www.ultimatesoftware.com/ContactUs).</span><span class="sxs-lookup"><span data-stu-id="beec9-173">tooconfigure single sign-on on **UltiPro** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[UltiPro support team](https://www.ultimatesoftware.com/ContactUs).</span></span>

> [!TIP]
> <span data-ttu-id="beec9-174">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="beec9-174">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="beec9-175">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="beec9-175">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="beec9-176">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="beec9-176">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="beec9-177">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="beec9-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="beec9-178">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="beec9-178">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="beec9-180">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="beec9-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="beec9-181">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="beec9-181">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="beec9-183">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="beec9-183">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="beec9-185">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="beec9-185">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="beec9-187">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="beec9-187">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ultipro-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="beec9-189">a.</span><span class="sxs-lookup"><span data-stu-id="beec9-189">a.</span></span> <span data-ttu-id="beec9-190">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="beec9-190">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="beec9-191">b.</span><span class="sxs-lookup"><span data-stu-id="beec9-191">b.</span></span> <span data-ttu-id="beec9-192">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="beec9-192">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="beec9-193">c.</span><span class="sxs-lookup"><span data-stu-id="beec9-193">c.</span></span> <span data-ttu-id="beec9-194">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="beec9-194">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="beec9-195">d.</span><span class="sxs-lookup"><span data-stu-id="beec9-195">d.</span></span> <span data-ttu-id="beec9-196">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="beec9-196">Click **Create**.</span></span>
 
### <a name="creating-a-ultipro-test-user"></a><span data-ttu-id="beec9-197">Vytvoření zkušebního uživatele UltiPro</span><span class="sxs-lookup"><span data-stu-id="beec9-197">Creating a UltiPro test user</span></span>

<span data-ttu-id="beec9-198">V této části vytvoříte volal Britta Simon v UltiPro uživatele.</span><span class="sxs-lookup"><span data-stu-id="beec9-198">In this section, you create a user called Britta Simon in UltiPro.</span></span> <span data-ttu-id="beec9-199">Práce s [tým podpory UltiPro](https://www.ultimatesoftware.com/ContactUs) pro přidání uživatelů hello hello UltiPro platformy.</span><span class="sxs-lookup"><span data-stu-id="beec9-199">Work with [UltiPro support team](https://www.ultimatesoftware.com/ContactUs) to add hello users in hello UltiPro platform.</span></span> <span data-ttu-id="beec9-200">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="beec9-200">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="beec9-201">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="beec9-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="beec9-202">V této části povolíte tak, že udělíte přístup tooUltiPro toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="beec9-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUltiPro.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="beec9-204">**tooassign Britta Simon tooUltiPro, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="beec9-204">**tooassign Britta Simon tooUltiPro, perform hello following steps:**</span></span>

1. <span data-ttu-id="beec9-205">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="beec9-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="beec9-207">V seznamu aplikace hello vyberte **UltiPro**.</span><span class="sxs-lookup"><span data-stu-id="beec9-207">In hello applications list, select **UltiPro**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ultipro-tutorial/tutorial_ultipro_app.png) 

3. <span data-ttu-id="beec9-209">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="beec9-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="beec9-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="beec9-211">Click **Add** button.</span></span> <span data-ttu-id="beec9-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="beec9-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="beec9-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="beec9-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="beec9-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="beec9-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="beec9-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="beec9-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="beec9-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="beec9-217">Testing single sign-on</span></span>

<span data-ttu-id="beec9-218">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="beec9-218">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="beec9-219">Když kliknete na dlaždici UltiPro hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour UltiPro aplikace.</span><span class="sxs-lookup"><span data-stu-id="beec9-219">When you click hello UltiPro tile in hello Access Panel, you should get automatically signed-on tooyour UltiPro application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="beec9-220">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="beec9-220">Additional resources</span></span>

* [<span data-ttu-id="beec9-221">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="beec9-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="beec9-222">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="beec9-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ultipro-tutorial/tutorial_general_203.png

