---
title: 'Kurz: Azure Active Directory integrace s Zendesk | Microsoft Docs'
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a služby Zendesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 46ccd57a4adeb810af459caaa1e592cf2b62cb8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a><span data-ttu-id="8e77b-103">Kurz: Azure Active Directory integrace s Zendesk</span><span class="sxs-lookup"><span data-stu-id="8e77b-103">Tutorial: Azure Active Directory integration with Zendesk</span></span>

<span data-ttu-id="8e77b-104">V tomto kurzu zjistíte, jak toointegrate Zendesk s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8e77b-104">In this tutorial, you learn how toointegrate Zendesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8e77b-105">Integrace služby Zendesk s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="8e77b-105">Integrating Zendesk with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8e77b-106">Můžete řídit ve službě Azure AD, který má přístup tooZendesk</span><span class="sxs-lookup"><span data-stu-id="8e77b-106">You can control in Azure AD who has access tooZendesk</span></span>
- <span data-ttu-id="8e77b-107">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooZendesk (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e77b-107">You can enable your users tooautomatically get signed-on tooZendesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8e77b-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8e77b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8e77b-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8e77b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e77b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8e77b-110">Prerequisites</span></span>

<span data-ttu-id="8e77b-111">tooconfigure integrace Azure AD s Zendesk, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8e77b-111">tooconfigure Azure AD integration with Zendesk, you need hello following items:</span></span>

- <span data-ttu-id="8e77b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e77b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8e77b-113">Této služby jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="8e77b-113">A Zendesk single sign-on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="8e77b-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="8e77b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="8e77b-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="8e77b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8e77b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="8e77b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8e77b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8e77b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="8e77b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="8e77b-118">Scenario description</span></span>
<span data-ttu-id="8e77b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="8e77b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8e77b-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="8e77b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8e77b-121">Přidání Zendesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8e77b-121">Adding Zendesk from hello gallery</span></span>
2. <span data-ttu-id="8e77b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e77b-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zendesk-from-hello-gallery"></a><span data-ttu-id="8e77b-123">Přidání Zendesk z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="8e77b-123">Adding Zendesk from hello gallery</span></span>
<span data-ttu-id="8e77b-124">tooconfigure hello integrace Zendesk do Azure AD, je nutné tooadd Zendesk hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="8e77b-124">tooconfigure hello integration of Zendesk into Azure AD, you need tooadd Zendesk from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8e77b-125">**tooadd Zendesk z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e77b-125">**tooadd Zendesk from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e77b-126">V hello  **[portálu Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8e77b-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8e77b-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8e77b-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="8e77b-131">Klikněte na tlačítko **novou aplikaci** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e77b-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="8e77b-133">Hello vyhledávacího pole zadejte **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-133">In hello search box, type **Zendesk**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. <span data-ttu-id="8e77b-135">Na panelu výsledků hello vyberte **Zendesk**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e77b-135">In hello results panel, select **Zendesk**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8e77b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e77b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8e77b-138">V této části konfiguraci a testování Azure AD jednotné přihlašování pomocí služby Zendesk podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="8e77b-138">In this section, you configure and test Azure AD single sign-on with Zendesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8e77b-139">Pro toowork jeden přihlašování Azure AD musí tooknow, jaké hello příslušného uživatele v této služby je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8e77b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zendesk is tooa user in Azure AD.</span></span> <span data-ttu-id="8e77b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v Zendesku musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="8e77b-140">In other words, a link relationship between an Azure AD user and hello related user in Zendesk needs toobe established.</span></span>

<span data-ttu-id="8e77b-141">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v této služby.</span><span class="sxs-lookup"><span data-stu-id="8e77b-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zendesk.</span></span>

<span data-ttu-id="8e77b-142">tooconfigure a testu Azure AD jednotné přihlašování s Zendesk, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="8e77b-142">tooconfigure and test Azure AD single sign-on with Zendesk, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8e77b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8e77b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8e77b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8e77b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8e77b-145">**[Vytvoření zkušebního uživatele Zendesk](#creating-a-zendesk-test-user)**  -toohave protějšek Britta Simon v této služby, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="8e77b-145">**[Creating a Zendesk test user](#creating-a-zendesk-test-user)** - toohave a counterpart of Britta Simon in Zendesk that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8e77b-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e77b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8e77b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="8e77b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8e77b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e77b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8e77b-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování v aplikaci služby Zendesk.</span><span class="sxs-lookup"><span data-stu-id="8e77b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zendesk application.</span></span>

<span data-ttu-id="8e77b-150">**tooconfigure Azure AD jednotné přihlašování s Zendesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e77b-150">**tooconfigure Azure AD single sign-on with Zendesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e77b-151">V portálu Azure, na hello hello **Zendesk** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-151">In hello Azure portal, on hello **Zendesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="8e77b-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e77b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. <span data-ttu-id="8e77b-155">Na hello **Zendesk domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8e77b-155">On hello **Zendesk Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    <span data-ttu-id="8e77b-157">a.</span><span class="sxs-lookup"><span data-stu-id="8e77b-157">a.</span></span> <span data-ttu-id="8e77b-158">V hello **přihlašovací adresa URL** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="8e77b-158">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<subdomain>.zendesk.com`</span></span>

    <span data-ttu-id="8e77b-159">b.</span><span class="sxs-lookup"><span data-stu-id="8e77b-159">b.</span></span> <span data-ttu-id="8e77b-160">V hello **identifikátor** textovému poli, typ hello hodnotu pomocí hello následující vzoru:`https://<subdomain>.zendesk.com`</span><span class="sxs-lookup"><span data-stu-id="8e77b-160">In hello **Identifier** textbox, type hello value using hello following pattern: `https://<subdomain>.zendesk.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8e77b-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="8e77b-161">These values are not real.</span></span> <span data-ttu-id="8e77b-162">Tyto hodnoty aktualizujte hello skutečné přihlašovací adresa URL a identifikátoru adresy URL.</span><span class="sxs-lookup"><span data-stu-id="8e77b-162">Update these values with hello actual Sign-on URL and Identifier URL.</span></span> <span data-ttu-id="8e77b-163">Obraťte se na [tým podpory služby Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8e77b-163">Contact [Zendesk support team](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) tooget these values.</span></span> 

4. <span data-ttu-id="8e77b-164">Zendesk očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="8e77b-164">Zendesk expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="8e77b-165">Neexistují žádné povinné atributy SAML, ale můžete volitelně přidat atribut z **uživatelské atributy** části podle následující hello následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="8e77b-165">There are no mandatory SAML attributes but optionally you can add an attribute from **User Attributes** section by following hello below steps:</span></span> 

     ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    <span data-ttu-id="8e77b-167">a.</span><span class="sxs-lookup"><span data-stu-id="8e77b-167">a.</span></span> <span data-ttu-id="8e77b-168">Klikněte na tlačítko hello **prohlížení a úpravy všechny ostatní atributy hello** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="8e77b-168">Click hello **View and edit all hello other attributes** check box.</span></span>
     
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    <span data-ttu-id="8e77b-170">b.</span><span class="sxs-lookup"><span data-stu-id="8e77b-170">b.</span></span> <span data-ttu-id="8e77b-171">Klikněte na tlačítko hello **přidat atribut** tooopen **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e77b-171">Click hello **Add Attribute** tooopen **Add attribute** dialog.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="8e77b-173">c.</span><span class="sxs-lookup"><span data-stu-id="8e77b-173">c.</span></span> <span data-ttu-id="8e77b-174">V hello **název** textovému poli, zadejte název atributu hello (například **emailaddress**).</span><span class="sxs-lookup"><span data-stu-id="8e77b-174">In hello **Name** textbox, type hello attribute name (for example **emailaddress**).</span></span>
    
    <span data-ttu-id="8e77b-175">d.</span><span class="sxs-lookup"><span data-stu-id="8e77b-175">d.</span></span> <span data-ttu-id="8e77b-176">Z hello **hodnotu** seznam, vyberte hello hodnota atributu (jako **user.mail**).</span><span class="sxs-lookup"><span data-stu-id="8e77b-176">From hello **Value** list, select hello attribute value (as **user.mail**).</span></span>
    
    <span data-ttu-id="8e77b-177">e.</span><span class="sxs-lookup"><span data-stu-id="8e77b-177">e.</span></span> <span data-ttu-id="8e77b-178">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="8e77b-178">Click **Ok**</span></span>
 
    > [!NOTE] 
    > <span data-ttu-id="8e77b-179">Můžete použít rozšíření atributy tooadd atributy, které nejsou ve službě Azure AD ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8e77b-179">You use extension attributes tooadd attributes that are not in Azure AD by default.</span></span> <span data-ttu-id="8e77b-180">Klikněte na tlačítko [atributy uživatele, které lze nastavit v SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello úplný seznam SAML atributy, které **Zendesk** přijímá.</span><span class="sxs-lookup"><span data-stu-id="8e77b-180">Click [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) tooget hello complete list of SAML attributes that **Zendesk** accepts.</span></span> 

5. <span data-ttu-id="8e77b-181">Na hello **SAML podpisový certifikát** část, kopie hello **kryptografický OTISK** hodnota certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8e77b-181">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. <span data-ttu-id="8e77b-183">Na hello **Zendesk konfigurace** klikněte na tlačítko **konfigurace Zendesk** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="8e77b-183">On hello **Zendesk Configuration** section, click **Configure Zendesk** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8e77b-184">Kopírování hello **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="8e77b-184">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. <span data-ttu-id="8e77b-186">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Zendesk.</span><span class="sxs-lookup"><span data-stu-id="8e77b-186">In a different web browser window, log into your Zendesk company site as an administrator.</span></span>

8. <span data-ttu-id="8e77b-187">Klikněte na tlačítko **správce**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-187">Click **Admin**.</span></span>

9. <span data-ttu-id="8e77b-188">V levém navigačním podokně hello, klikněte na **nastavení**a potom klikněte na **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-188">In hello left navigation pane, click **Settings**, and then click **Security**.</span></span>

10. <span data-ttu-id="8e77b-189">Na hello **zabezpečení** proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8e77b-189">On hello **Security** page, perform hello following steps:</span></span> 
   
     <span data-ttu-id="8e77b-190">![Zabezpečení](./media/active-directory-saas-zendesk-tutorial/ic773089.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="8e77b-190">![Security](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Security")</span></span>

    <span data-ttu-id="8e77b-191">![Jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/ic773090.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="8e77b-191">![Single sign-on](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Single sign-on")</span></span>

     <span data-ttu-id="8e77b-192">a.</span><span class="sxs-lookup"><span data-stu-id="8e77b-192">a.</span></span> <span data-ttu-id="8e77b-193">Klikněte na tlačítko hello **správce & agenty** kartě.</span><span class="sxs-lookup"><span data-stu-id="8e77b-193">Click hello **Admin & Agents** tab.</span></span>

     <span data-ttu-id="8e77b-194">b.</span><span class="sxs-lookup"><span data-stu-id="8e77b-194">b.</span></span> <span data-ttu-id="8e77b-195">Vyberte **jednotné přihlašování (SSO) a SAML**a potom vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-195">Select **Single sign-on (SSO) and SAML**, and then select **SAML**.</span></span>

     <span data-ttu-id="8e77b-196">c.</span><span class="sxs-lookup"><span data-stu-id="8e77b-196">c.</span></span> <span data-ttu-id="8e77b-197">V **URL jednotné přihlašování SAML** textovému poli, vložte hodnotu hello **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8e77b-197">In **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

     <span data-ttu-id="8e77b-198">d.</span><span class="sxs-lookup"><span data-stu-id="8e77b-198">d.</span></span> <span data-ttu-id="8e77b-199">V **vzdálené adresy URL odhlašovací** textovému poli, vložte hodnotu hello **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8e77b-199">In **Remote Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
        
     <span data-ttu-id="8e77b-200">e.</span><span class="sxs-lookup"><span data-stu-id="8e77b-200">e.</span></span> <span data-ttu-id="8e77b-201">V **otisků prstů certifikátů** textovému poli, vložte hello **kryptografický otisk** hodnota certifikát, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8e77b-201">In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="8e77b-202">f.</span><span class="sxs-lookup"><span data-stu-id="8e77b-202">f.</span></span> <span data-ttu-id="8e77b-203">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-203">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8e77b-204">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e77b-204">Creating an Azure AD test user</span></span>
<span data-ttu-id="8e77b-205">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="8e77b-205">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="8e77b-207">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e77b-207">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e77b-208">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="8e77b-208">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8e77b-210">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-210">toodisplay hello list of users go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8e77b-212">V horní části hello hello dialogového okna, klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e77b-212">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8e77b-214">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8e77b-214">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8e77b-216">a.</span><span class="sxs-lookup"><span data-stu-id="8e77b-216">a.</span></span> <span data-ttu-id="8e77b-217">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-217">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8e77b-218">b.</span><span class="sxs-lookup"><span data-stu-id="8e77b-218">b.</span></span> <span data-ttu-id="8e77b-219">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8e77b-219">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8e77b-220">c.</span><span class="sxs-lookup"><span data-stu-id="8e77b-220">c.</span></span> <span data-ttu-id="8e77b-221">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-221">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8e77b-222">d.</span><span class="sxs-lookup"><span data-stu-id="8e77b-222">d.</span></span> <span data-ttu-id="8e77b-223">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-223">Click **Create**.</span></span> 

### <a name="creating-a-zendesk-test-user"></a><span data-ttu-id="8e77b-224">Vytvoření zkušebního uživatele Zendesk</span><span class="sxs-lookup"><span data-stu-id="8e77b-224">Creating a Zendesk test user</span></span>

<span data-ttu-id="8e77b-225">Uživatelé toolog tooenable Azure AD do **Zendesk**, musí být zřízená do **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-225">tooenable Azure AD users toolog into **Zendesk**, they must be provisioned into **Zendesk**.</span></span>  
<span data-ttu-id="8e77b-226">V závislosti na roli hello přiřazené v aplikacích hello jeho hello očekávané chování:</span><span class="sxs-lookup"><span data-stu-id="8e77b-226">Depending on hello role assigned in hello apps, it's hello expected behavior:</span></span>

 1. <span data-ttu-id="8e77b-227">**Koncový uživatel** účty jsou zřízené automaticky při přihlášení.</span><span class="sxs-lookup"><span data-stu-id="8e77b-227">**End-user** accounts are automatically provisioned when signing in.</span></span>
 2. <span data-ttu-id="8e77b-228">**Agent** a **správce** nutné účty toobe ručně zřízené v **Zendesk** před přihlášením.</span><span class="sxs-lookup"><span data-stu-id="8e77b-228">**Agent** and **Admin** accounts need toobe manually provisioned in **Zendesk** before signing in.</span></span>
 
<span data-ttu-id="8e77b-229">**tooprovision uživatelský účet, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e77b-229">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e77b-230">Přihlaste se tooyour **Zendesk** klienta.</span><span class="sxs-lookup"><span data-stu-id="8e77b-230">Log in tooyour **Zendesk** tenant.</span></span>

2. <span data-ttu-id="8e77b-231">Vyberte hello **seznamu zákazníků** kartě.</span><span class="sxs-lookup"><span data-stu-id="8e77b-231">Select hello **Customer List** tab.</span></span>

3. <span data-ttu-id="8e77b-232">Vyberte hello **uživatele** a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-232">Select hello **User** tab, and click **Add**.</span></span>
   
    <span data-ttu-id="8e77b-233">![Přidat uživatele](./media/active-directory-saas-zendesk-tutorial/ic773632.png "přidat uživatele")</span><span class="sxs-lookup"><span data-stu-id="8e77b-233">![Add user](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Add user")</span></span>
4. <span data-ttu-id="8e77b-234">Zadejte e-mailovou adresu hello existující účet služby Azure AD tooprovision a pak klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-234">Type hello email address of an existing Azure AD account you want tooprovision, and then click **Save**.</span></span>
   
    <span data-ttu-id="8e77b-235">![Nový uživatel](./media/active-directory-saas-zendesk-tutorial/ic773633.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="8e77b-235">![New user](./media/active-directory-saas-zendesk-tutorial/ic773633.png "New user")</span></span>

> [!NOTE]
> <span data-ttu-id="8e77b-236">Můžete použít všechny ostatní Zendesk uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Zendesk tooprovision AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="8e77b-236">You can use any other Zendesk user account creation tools or APIs provided by Zendesk tooprovision AAD user accounts.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8e77b-237">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="8e77b-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8e77b-238">V této části povolíte tak, že udělíte přístup tooZendesk toouse Britta Simon Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="8e77b-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZendesk.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="8e77b-240">**tooassign Britta Simon tooZendesk, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="8e77b-240">**tooassign Britta Simon tooZendesk, perform hello following steps:**</span></span>

1. <span data-ttu-id="8e77b-241">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="8e77b-243">V seznamu aplikace hello vyberte **Zendesk**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-243">In hello applications list, select **Zendesk**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. <span data-ttu-id="8e77b-245">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="8e77b-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="8e77b-247">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8e77b-247">Click **Add** button.</span></span> <span data-ttu-id="8e77b-248">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e77b-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="8e77b-250">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="8e77b-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8e77b-251">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e77b-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8e77b-252">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="8e77b-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8e77b-253">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="8e77b-253">Testing single sign-on</span></span>

<span data-ttu-id="8e77b-254">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="8e77b-254">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8e77b-255">Když kliknete na dlaždici služby Zendesk hello v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour Zendesk aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e77b-255">When you click hello Zendesk tile in hello Access Panel, you should get automatically signed-on tooyour Zendesk application.</span></span>
<span data-ttu-id="8e77b-256">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8e77b-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e77b-257">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="8e77b-257">Additional resources</span></span>

* [<span data-ttu-id="8e77b-258">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8e77b-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8e77b-259">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8e77b-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png
