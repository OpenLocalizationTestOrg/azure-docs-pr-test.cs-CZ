---
title: 'Kurz: Azure Active Directory integrace s Sugar CRM | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a Sugar CRM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: 108d2f8125e410743ee7bc48883a1d0b00602615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a><span data-ttu-id="7f807-103">Kurz: Azure Active Directory integrace s Sugar CRM</span><span class="sxs-lookup"><span data-stu-id="7f807-103">Tutorial: Azure Active Directory integration with Sugar CRM</span></span>

<span data-ttu-id="7f807-104">V tomto kurzu zjistíte, jak toointegrate Sugar CRM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7f807-104">In this tutorial, you learn how toointegrate Sugar CRM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7f807-105">Integrace s Azure AD Sugar CRM poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="7f807-105">Integrating Sugar CRM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7f807-106">Můžete řídit ve službě Azure AD, který má přístup tooSugar CRM</span><span class="sxs-lookup"><span data-stu-id="7f807-106">You can control in Azure AD who has access tooSugar CRM</span></span>
- <span data-ttu-id="7f807-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSugar CRM (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f807-107">You can enable your users tooautomatically get signed-on tooSugar CRM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7f807-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7f807-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7f807-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7f807-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f807-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="7f807-110">Prerequisites</span></span>

<span data-ttu-id="7f807-111">tooconfigure integrace Azure AD s Sugar CRM, budete potřebovat hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="7f807-111">tooconfigure Azure AD integration with Sugar CRM, you need hello following items:</span></span>

- <span data-ttu-id="7f807-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f807-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7f807-113">Sugar CRM jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="7f807-113">A Sugar CRM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7f807-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f807-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7f807-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="7f807-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7f807-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="7f807-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7f807-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7f807-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7f807-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="7f807-118">Scenario description</span></span>
<span data-ttu-id="7f807-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="7f807-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7f807-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="7f807-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7f807-121">Přidání Sugar CRM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7f807-121">Adding Sugar CRM from hello gallery</span></span>
2. <span data-ttu-id="7f807-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f807-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sugar-crm-from-hello-gallery"></a><span data-ttu-id="7f807-123">Přidání Sugar CRM z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="7f807-123">Adding Sugar CRM from hello gallery</span></span>
<span data-ttu-id="7f807-124">tooconfigure hello integrace Sugar CRM do Azure AD, je nutné tooadd Sugar CRM hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="7f807-124">tooconfigure hello integration of Sugar CRM into Azure AD, you need tooadd Sugar CRM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7f807-125">**tooadd Sugar CRM z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f807-125">**tooadd Sugar CRM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f807-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f807-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7f807-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="7f807-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7f807-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f807-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="7f807-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f807-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="7f807-133">Hello vyhledávacího pole zadejte **Sugar CRM**.</span><span class="sxs-lookup"><span data-stu-id="7f807-133">In hello search box, type **Sugar CRM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. <span data-ttu-id="7f807-135">Na panelu výsledků hello vyberte **Sugar CRM**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f807-135">In hello results panel, select **Sugar CRM**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7f807-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f807-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7f807-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sugar CRM podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="7f807-138">In this section, you configure and test Azure AD single sign-on with Sugar CRM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7f807-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v Sugar CRM je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7f807-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sugar CRM is tooa user in Azure AD.</span></span> <span data-ttu-id="7f807-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Sugar CRM musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="7f807-140">In other words, a link relationship between an Azure AD user and hello related user in Sugar CRM needs toobe established.</span></span>

<span data-ttu-id="7f807-141">V Sugar CRM přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="7f807-141">In Sugar CRM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7f807-142">tooconfigure a testu Azure AD jednotné přihlašování s Sugar CRM, budete potřebovat následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="7f807-142">tooconfigure and test Azure AD single sign-on with Sugar CRM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7f807-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="7f807-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7f807-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7f807-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7f807-145">**[Vytvoření zkušebního uživatele Sugar CRM](#creating-a-sugar-crm-test-user)**  -toohave protějšek Britta Simon v CRM Sugar, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="7f807-145">**[Creating a Sugar CRM test user](#creating-a-sugar-crm-test-user)** - toohave a counterpart of Britta Simon in Sugar CRM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7f807-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f807-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7f807-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="7f807-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7f807-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f807-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7f807-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci Sugar CRM.</span><span class="sxs-lookup"><span data-stu-id="7f807-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sugar CRM application.</span></span>

<span data-ttu-id="7f807-150">**tooconfigure Azure AD jednotné přihlašování s Sugar CRM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f807-150">**tooconfigure Azure AD single sign-on with Sugar CRM, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f807-151">V portálu Azure, na hello hello **Sugar CRM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="7f807-151">In hello Azure portal, on hello **Sugar CRM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="7f807-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f807-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. <span data-ttu-id="7f807-155">Na hello **Sugar CRM domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f807-155">On hello **Sugar CRM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    <span data-ttu-id="7f807-157">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="7f807-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > <span data-ttu-id="7f807-158">Hodnota Hello není skutečné.</span><span class="sxs-lookup"><span data-stu-id="7f807-158">hello value is not real.</span></span> <span data-ttu-id="7f807-159">Aktualizace hello hodnotu s hello skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f807-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="7f807-160">Obraťte se na [tým podpory klienta CRM Sugar](https://support.sugarcrm.com/) tooget hello hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7f807-160">Contact [Sugar CRM Client support team](https://support.sugarcrm.com/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="7f807-161">Na hello **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="7f807-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. <span data-ttu-id="7f807-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f807-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7f807-165">Na hello **Sugar CRM konfigurace** klikněte na tlačítko **konfigurace CRM Sugar** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="7f807-165">On hello **Sugar CRM Configuration** section, click **Configure Sugar CRM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7f807-166">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="7f807-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. <span data-ttu-id="7f807-168">V okně prohlížeče jiný web Přihlaste se jako správce na webu společnosti tooyour Sugar CRM.</span><span class="sxs-lookup"><span data-stu-id="7f807-168">In a different web browser window, log in tooyour Sugar CRM company site as an administrator.</span></span>

8. <span data-ttu-id="7f807-169">Přejděte příliš**správce**.</span><span class="sxs-lookup"><span data-stu-id="7f807-169">Go too**Admin**.</span></span>
   
    <span data-ttu-id="7f807-170">![Správce](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "správce")</span><span class="sxs-lookup"><span data-stu-id="7f807-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

9. <span data-ttu-id="7f807-171">V hello **správy** klikněte na tlačítko **Správa hesel**.</span><span class="sxs-lookup"><span data-stu-id="7f807-171">In hello **Administration** section, click **Password Management**.</span></span>
   
    <span data-ttu-id="7f807-172">![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "správy")</span><span class="sxs-lookup"><span data-stu-id="7f807-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span></span>

10. <span data-ttu-id="7f807-173">Vyberte **povolit ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="7f807-173">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="7f807-174">![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "správy")</span><span class="sxs-lookup"><span data-stu-id="7f807-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span></span>

11. <span data-ttu-id="7f807-175">V hello **ověřování SAML** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f807-175">In hello **SAML Authentication** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="7f807-176">![Ověřování SAML](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "ověřování SAML")</span><span class="sxs-lookup"><span data-stu-id="7f807-176">![SAML Authentication](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML Authentication")</span></span>  
 
    <span data-ttu-id="7f807-177">a.</span><span class="sxs-lookup"><span data-stu-id="7f807-177">a.</span></span> <span data-ttu-id="7f807-178">V hello **přihlašovací adresa URL** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7f807-178">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="7f807-179">b.</span><span class="sxs-lookup"><span data-stu-id="7f807-179">b.</span></span> <span data-ttu-id="7f807-180">V hello **SLO URL** textovému poli, vložte hodnotu hello **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7f807-180">In hello **SLO URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="7f807-181">c.</span><span class="sxs-lookup"><span data-stu-id="7f807-181">c.</span></span> <span data-ttu-id="7f807-182">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku hello kopírování obsahu ho do schránky a potom vložte hello celý certifikát do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="7f807-182">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="7f807-183">d.</span><span class="sxs-lookup"><span data-stu-id="7f807-183">d.</span></span> <span data-ttu-id="7f807-184">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7f807-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7f807-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="7f807-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7f807-186">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="7f807-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7f807-187">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7f807-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7f807-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="7f807-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="7f807-189">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="7f807-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="7f807-191">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f807-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f807-192">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="7f807-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7f807-194">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="7f807-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7f807-196">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="7f807-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7f807-198">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="7f807-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7f807-200">a.</span><span class="sxs-lookup"><span data-stu-id="7f807-200">a.</span></span> <span data-ttu-id="7f807-201">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7f807-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7f807-202">b.</span><span class="sxs-lookup"><span data-stu-id="7f807-202">b.</span></span> <span data-ttu-id="7f807-203">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7f807-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7f807-204">c.</span><span class="sxs-lookup"><span data-stu-id="7f807-204">c.</span></span> <span data-ttu-id="7f807-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7f807-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7f807-206">d.</span><span class="sxs-lookup"><span data-stu-id="7f807-206">d.</span></span> <span data-ttu-id="7f807-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7f807-207">Click **Create**.</span></span>
 
### <a name="creating-a-sugar-crm-test-user"></a><span data-ttu-id="7f807-208">Vytvoření zkušebního uživatele Sugar CRM</span><span class="sxs-lookup"><span data-stu-id="7f807-208">Creating a Sugar CRM test user</span></span>

<span data-ttu-id="7f807-209">V pořadí tooenable Azure AD Uživatelé toolog v tooSugar CRM musí být zřízená tooSugar CRM.</span><span class="sxs-lookup"><span data-stu-id="7f807-209">In order tooenable Azure AD users toolog in tooSugar CRM, they must be provisioned tooSugar CRM.</span></span>

<span data-ttu-id="7f807-210">V případě hello aplikace Sugar CRM je zřizování ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="7f807-210">In hello case of Sugar CRM, provisioning is a manual task.</span></span>

<span data-ttu-id="7f807-211">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f807-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f807-212">Přihlaste se tooyour **Sugar CRM** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="7f807-212">Log in tooyour **Sugar CRM** company site as administrator.</span></span>

2. <span data-ttu-id="7f807-213">Přejděte příliš**správce**.</span><span class="sxs-lookup"><span data-stu-id="7f807-213">Go too**Admin**.</span></span>
   
    <span data-ttu-id="7f807-214">![Správce](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "správce")</span><span class="sxs-lookup"><span data-stu-id="7f807-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

3. <span data-ttu-id="7f807-215">V hello **správy** klikněte na tlačítko **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="7f807-215">In hello **Administration** section, click **User Management**.</span></span>
   
    <span data-ttu-id="7f807-216">![Správa](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "správy")</span><span class="sxs-lookup"><span data-stu-id="7f807-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span></span>

4. <span data-ttu-id="7f807-217">Přejděte příliš**uživatelé \> vytvořit nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="7f807-217">Go too**Users \> Create New User**.</span></span>
   
    <span data-ttu-id="7f807-218">![Vytvoření nového uživatele](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "vytvořit nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="7f807-218">![Create New User](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Create New User")</span></span>

5. <span data-ttu-id="7f807-219">Na hello **profil uživatele** kartu, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f807-219">On hello **User Profile** tab, perform hello following steps:</span></span>
   
    <span data-ttu-id="7f807-220">![Nový uživatel](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="7f807-220">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "New User")</span></span>

    <span data-ttu-id="7f807-221">a.</span><span class="sxs-lookup"><span data-stu-id="7f807-221">a.</span></span> <span data-ttu-id="7f807-222">Typ hello **uživatelské jméno**, **příjmení**, a **e-mailová adresa** platný uživatele Azure Active Directory do hello související textových polí.</span><span class="sxs-lookup"><span data-stu-id="7f807-222">Type hello **user name**, **last name**, and **email address** of a valid Azure Active Directory user into hello related textboxes.</span></span>
  
6. <span data-ttu-id="7f807-223">Jako **stav**, vyberte **Active**.</span><span class="sxs-lookup"><span data-stu-id="7f807-223">As **Status**, select **Active**.</span></span>

7. <span data-ttu-id="7f807-224">Na kartě heslo hello proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="7f807-224">On hello Password tab, perform hello following steps:</span></span>
   
    <span data-ttu-id="7f807-225">![Nový uživatel](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="7f807-225">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "New User")</span></span>

    <span data-ttu-id="7f807-226">a.</span><span class="sxs-lookup"><span data-stu-id="7f807-226">a.</span></span> <span data-ttu-id="7f807-227">Zadejte heslo hello do hello související textové pole.</span><span class="sxs-lookup"><span data-stu-id="7f807-227">Type hello password into hello related textbox.</span></span>

    <span data-ttu-id="7f807-228">b.</span><span class="sxs-lookup"><span data-stu-id="7f807-228">b.</span></span> <span data-ttu-id="7f807-229">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="7f807-229">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="7f807-230">Můžete použít všechny ostatní Sugar CRM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Sugar CRM tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="7f807-230">You can use any other Sugar CRM user account creation tools or APIs provided by Sugar CRM tooprovision AAD user accounts.</span></span> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7f807-231">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="7f807-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7f807-232">V této části povolíte tak, že udělíte přístup tooSugar CRM Britta Simon toouse Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="7f807-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSugar CRM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="7f807-234">**tooassign tooSugar Britta Simon CRM, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="7f807-234">**tooassign Britta Simon tooSugar CRM, perform hello following steps:**</span></span>

1. <span data-ttu-id="7f807-235">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="7f807-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="7f807-237">V seznamu aplikace hello vyberte **Sugar CRM**.</span><span class="sxs-lookup"><span data-stu-id="7f807-237">In hello applications list, select **Sugar CRM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. <span data-ttu-id="7f807-239">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="7f807-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="7f807-241">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="7f807-241">Click **Add** button.</span></span> <span data-ttu-id="7f807-242">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f807-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="7f807-244">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="7f807-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7f807-245">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f807-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7f807-246">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="7f807-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7f807-247">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="7f807-247">Testing single sign-on</span></span>

<span data-ttu-id="7f807-248">Hello cílem této části je tootest pomocí Azure AD konfigurace přihlášení hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="7f807-248">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7f807-249">Když kliknete na dlaždici Sugar CRM hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Sugar CRM aplikace.</span><span class="sxs-lookup"><span data-stu-id="7f807-249">When you click hello Sugar CRM tile in hello Access Panel, you should get automatically signed-on tooyour Sugar CRM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f807-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="7f807-250">Additional resources</span></span>

* [<span data-ttu-id="7f807-251">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7f807-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7f807-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7f807-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

