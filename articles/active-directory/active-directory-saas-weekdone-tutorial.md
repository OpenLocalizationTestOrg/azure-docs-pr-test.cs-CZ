---
title: 'Kurz: Azure Active Directory integrace s Weekdone | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Weekdone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f997f1b49ff1aa0659a2409fdd945c6f96413b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a><span data-ttu-id="cb261-103">Kurz: Azure Active Directory integrace s Weekdone</span><span class="sxs-lookup"><span data-stu-id="cb261-103">Tutorial: Azure Active Directory integration with Weekdone</span></span>

<span data-ttu-id="cb261-104">V tomto kurzu zjistíte, jak toointegrate Weekdone s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cb261-104">In this tutorial, you learn how toointegrate Weekdone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cb261-105">Integrace Weekdone s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cb261-105">Integrating Weekdone with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cb261-106">Můžete řídit ve službě Azure AD, který má přístup tooWeekdone</span><span class="sxs-lookup"><span data-stu-id="cb261-106">You can control in Azure AD who has access tooWeekdone</span></span>
- <span data-ttu-id="cb261-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooWeekdone (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb261-107">You can enable your users tooautomatically get signed-on tooWeekdone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cb261-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cb261-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cb261-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cb261-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb261-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cb261-110">Prerequisites</span></span>

<span data-ttu-id="cb261-111">Integrace služby Azure AD s Weekdone tooconfigure, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="cb261-111">tooconfigure Azure AD integration with Weekdone, you need hello following items:</span></span>

- <span data-ttu-id="cb261-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb261-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cb261-113">Weekdone jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cb261-113">A Weekdone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cb261-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cb261-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cb261-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cb261-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cb261-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cb261-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cb261-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cb261-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cb261-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cb261-118">Scenario description</span></span>
<span data-ttu-id="cb261-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cb261-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cb261-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cb261-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cb261-121">Přidání Weekdone z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cb261-121">Adding Weekdone from hello gallery</span></span>
2. <span data-ttu-id="cb261-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb261-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-weekdone-from-hello-gallery"></a><span data-ttu-id="cb261-123">Přidání Weekdone z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="cb261-123">Adding Weekdone from hello gallery</span></span>
<span data-ttu-id="cb261-124">tooconfigure hello integrace Weekdone do Azure AD, je nutné tooadd Weekdone hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cb261-124">tooconfigure hello integration of Weekdone into Azure AD, you need tooadd Weekdone from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cb261-125">**tooadd Weekdone z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb261-125">**tooadd Weekdone from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb261-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb261-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cb261-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cb261-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cb261-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb261-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cb261-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cb261-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cb261-133">Hello vyhledávacího pole zadejte **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="cb261-133">In hello search box, type **Weekdone**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_search.png)

5. <span data-ttu-id="cb261-135">Na panelu výsledků hello vyberte **Weekdone**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb261-135">In hello results panel, select **Weekdone**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cb261-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb261-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cb261-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Weekdone podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cb261-138">In this section, you configure and test Azure AD single sign-on with Weekdone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cb261-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v Weekdone je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cb261-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Weekdone is tooa user in Azure AD.</span></span> <span data-ttu-id="cb261-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Weekdone musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="cb261-140">In other words, a link relationship between an Azure AD user and hello related user in Weekdone needs toobe established.</span></span>

<span data-ttu-id="cb261-141">V Weekdone, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="cb261-141">In Weekdone, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cb261-142">tooconfigure a testu Azure AD jednotné přihlašování s Weekdone, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="cb261-142">tooconfigure and test Azure AD single sign-on with Weekdone, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cb261-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="cb261-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cb261-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cb261-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cb261-145">**[Vytvoření zkušebního uživatele Weekdone](#creating-a-weekdone-test-user)**  -toohave protějšek Britta Simon v Weekdone, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb261-145">**[Creating a Weekdone test user](#creating-a-weekdone-test-user)** - toohave a counterpart of Britta Simon in Weekdone that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cb261-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb261-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cb261-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="cb261-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cb261-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb261-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cb261-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Weekdone.</span><span class="sxs-lookup"><span data-stu-id="cb261-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Weekdone application.</span></span>

<span data-ttu-id="cb261-150">**tooconfigure Azure AD jednotné přihlašování s Weekdone, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb261-150">**tooconfigure Azure AD single sign-on with Weekdone, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb261-151">V portálu Azure, na hello hello **Weekdone** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cb261-151">In hello Azure portal, on hello **Weekdone** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cb261-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb261-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_samlbase.png)

3. <span data-ttu-id="cb261-155">Na hello **Weekdone domény a adresy URL** část, pokud chcete aplikace hello tooconfigure v **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="cb261-155">On hello **Weekdone Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url1.png)

    <span data-ttu-id="cb261-157">a.</span><span class="sxs-lookup"><span data-stu-id="cb261-157">a.</span></span> <span data-ttu-id="cb261-158">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="cb261-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

    <span data-ttu-id="cb261-159">b.</span><span class="sxs-lookup"><span data-stu-id="cb261-159">b.</span></span> <span data-ttu-id="cb261-160">V hello **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="cb261-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

4. <span data-ttu-id="cb261-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="cb261-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="cb261-162">Pokud chcete aplikace hello tooconfigure v **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="cb261-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url2.png)

    <span data-ttu-id="cb261-164">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="cb261-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://weekdone.com/a/<tenantname>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="cb261-165">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="cb261-165">These values are not real.</span></span> <span data-ttu-id="cb261-166">Aktualizovat tyto hodnoty s hello skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="cb261-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="cb261-167">Obraťte se na [tým podpory Weekdone klienta](mailto:hello@weekdone.com) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cb261-167">Contact [Weekdone Client support team](mailto:hello@weekdone.com) tooget these values.</span></span> 

5. <span data-ttu-id="cb261-168">Na hello **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cb261-168">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_certificate.png) 

6. <span data-ttu-id="cb261-170">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cb261-170">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="cb261-172">Na hello **Weekdone konfigurace** klikněte na tlačítko **konfigurace Weekdone** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cb261-172">On hello **Weekdone Configuration** section, click **Configure Weekdone** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="cb261-173">Kopírování hello **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cb261-173">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_configure.png) 

8. <span data-ttu-id="cb261-175">tooconfigure jednotného přihlašování na **Weekdone** straně, je nutné stáhnout hello toosend **soubor XML s metadaty, adresa URL Sign-Out, SAML Entity ID a SAML jeden přihlašování adresa URL služby** příliš[Weekdone podpory tým](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="cb261-175">tooconfigure single sign-on on **Weekdone** side, you need toosend hello downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Weekdone support team](mailto:hello@weekdone.com).</span></span>

> [!TIP]
> <span data-ttu-id="cb261-176">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="cb261-176">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cb261-177">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="cb261-177">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cb261-178">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cb261-178">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cb261-179">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb261-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="cb261-180">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="cb261-180">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cb261-182">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb261-182">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb261-183">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cb261-183">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cb261-185">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cb261-185">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cb261-187">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="cb261-187">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cb261-189">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cb261-189">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cb261-191">a.</span><span class="sxs-lookup"><span data-stu-id="cb261-191">a.</span></span> <span data-ttu-id="cb261-192">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cb261-192">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cb261-193">b.</span><span class="sxs-lookup"><span data-stu-id="cb261-193">b.</span></span> <span data-ttu-id="cb261-194">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cb261-194">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cb261-195">c.</span><span class="sxs-lookup"><span data-stu-id="cb261-195">c.</span></span> <span data-ttu-id="cb261-196">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cb261-196">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cb261-197">d.</span><span class="sxs-lookup"><span data-stu-id="cb261-197">d.</span></span> <span data-ttu-id="cb261-198">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cb261-198">Click **Create**.</span></span>
 
### <a name="creating-a-weekdone-test-user"></a><span data-ttu-id="cb261-199">Vytvoření zkušebního uživatele Weekdone</span><span class="sxs-lookup"><span data-stu-id="cb261-199">Creating a Weekdone test user</span></span>

<span data-ttu-id="cb261-200">Hello cílem této části je toocreate volal Britta Simon v Weekdone uživatele.</span><span class="sxs-lookup"><span data-stu-id="cb261-200">hello objective of this section is toocreate a user called Britta Simon in Weekdone.</span></span> <span data-ttu-id="cb261-201">Weekdone podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="cb261-201">Weekdone supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="cb261-202">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="cb261-202">There is no action item for you in this section.</span></span> <span data-ttu-id="cb261-203">Pokud ještě neexistuje, vytvoří se nový uživatel během pokusu o tooaccess Weekdone.</span><span class="sxs-lookup"><span data-stu-id="cb261-203">A new user is created during an attempt tooaccess Weekdone if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="cb261-204">Pokud potřebujete toocreate uživatel ručně, je nutné toocontact hello [tým podpory Weekdone klienta](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="cb261-204">If you need toocreate a user manually, you need toocontact hello [Weekdone Client support team](mailto:hello@weekdone.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cb261-205">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="cb261-205">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cb261-206">V této části povolíte tak, že udělíte přístup tooWeekdone toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cb261-206">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWeekdone.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cb261-208">**tooassign Britta Simon tooWeekdone, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="cb261-208">**tooassign Britta Simon tooWeekdone, perform hello following steps:**</span></span>

1. <span data-ttu-id="cb261-209">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb261-209">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cb261-211">V seznamu aplikace hello vyberte **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="cb261-211">In hello applications list, select **Weekdone**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_app.png) 

3. <span data-ttu-id="cb261-213">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cb261-213">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cb261-215">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cb261-215">Click **Add** button.</span></span> <span data-ttu-id="cb261-216">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb261-216">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cb261-218">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="cb261-218">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cb261-219">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb261-219">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cb261-220">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cb261-220">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cb261-221">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cb261-221">Testing single sign-on</span></span>

<span data-ttu-id="cb261-222">Hello cílem této části je tootest pomocí konfigurace Azure AD jednotného přihlašování k přístupovému panelu hello.</span><span class="sxs-lookup"><span data-stu-id="cb261-222">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="cb261-223">Když kliknete na dlaždici Weekdone hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Weekdone aplikace.</span><span class="sxs-lookup"><span data-stu-id="cb261-223">When you click hello Weekdone tile in hello Access Panel, you should get automatically signed-on tooyour Weekdone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cb261-224">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cb261-224">Additional resources</span></span>

* [<span data-ttu-id="cb261-225">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb261-225">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cb261-226">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cb261-226">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png

