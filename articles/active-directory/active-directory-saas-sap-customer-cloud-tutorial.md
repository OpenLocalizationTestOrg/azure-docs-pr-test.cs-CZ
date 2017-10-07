---
title: "Kurz: Azure Active Directory integrace s cloudem SAP zákazníka. | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP cloudu pro zákazníka."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 0525ea81122458ab3ac24a5bdb0b5f628405dd05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a><span data-ttu-id="c6132-103">Kurz: Azure Active Directory integrace s cloudem SAP zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c6132-103">Tutorial: Azure Active Directory integration with SAP Cloud for Customer</span></span>

<span data-ttu-id="c6132-104">V tomto kurzu zjistíte, jak toointegrate SAP cloudu pro zákazníka s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c6132-104">In this tutorial, you learn how toointegrate SAP Cloud for Customer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c6132-105">Integrace SAP cloudu pro zákazníka s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c6132-105">Integrating SAP Cloud for Customer with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c6132-106">Můžete řídit ve službě Azure AD, který má přístup tooSAP cloudu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c6132-106">You can control in Azure AD who has access tooSAP Cloud for Customer</span></span>
- <span data-ttu-id="c6132-107">Vaši uživatelé tooautomatically get přihlášeného tooSAP cloudu můžete povolit pro zákazníka (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="c6132-107">You can enable your users tooautomatically get signed-on tooSAP Cloud for Customer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c6132-108">Můžete spravovat vaše účty v jednom centrálním místě - hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c6132-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c6132-109">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c6132-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c6132-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c6132-110">Prerequisites</span></span>

<span data-ttu-id="c6132-111">tooconfigure integrace Azure AD s SAP cloudu pro zákazníka, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="c6132-111">tooconfigure Azure AD integration with SAP Cloud for Customer, you need hello following items:</span></span>

- <span data-ttu-id="c6132-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c6132-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c6132-113">Cloud SAP pro zákazníka jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="c6132-113">A SAP Cloud for Customer single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c6132-114">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c6132-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c6132-115">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c6132-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c6132-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c6132-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c6132-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c6132-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c6132-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c6132-118">Scenario description</span></span>
<span data-ttu-id="c6132-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c6132-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c6132-120">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c6132-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c6132-121">Přidání SAP cloudu pro zákazníka z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c6132-121">Adding SAP Cloud for Customer from hello gallery</span></span>
2. <span data-ttu-id="c6132-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c6132-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-cloud-for-customer-from-hello-gallery"></a><span data-ttu-id="c6132-123">Přidání SAP cloudu pro zákazníka z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="c6132-123">Adding SAP Cloud for Customer from hello gallery</span></span>
<span data-ttu-id="c6132-124">tooconfigure hello integrace SAP cloudu pro zákazníka do služby Azure AD, musíte pro zákazníka hello Galerie tooyour seznamu spravovaných aplikací SaaS tooadd SAP cloudu.</span><span class="sxs-lookup"><span data-stu-id="c6132-124">tooconfigure hello integration of SAP Cloud for Customer into Azure AD, you need tooadd SAP Cloud for Customer from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c6132-125">**tooadd SAP cloudu pro zákazníka z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c6132-125">**tooadd SAP Cloud for Customer from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6132-126">V hello  **[portál Azure](https://portal.azure.com)**, na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c6132-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c6132-128">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c6132-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c6132-129">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c6132-129">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="c6132-131">tooadd novou aplikaci, klikněte na tlačítko **novou aplikaci** hello nahoře dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6132-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="c6132-133">Hello vyhledávacího pole zadejte **SAP cloudu pro zákazníka**.</span><span class="sxs-lookup"><span data-stu-id="c6132-133">In hello search box, type **SAP Cloud for Customer**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. <span data-ttu-id="c6132-135">Na panelu výsledků hello vyberte **SAP cloudu pro zákazníka**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="c6132-135">In hello results panel, select **SAP Cloud for Customer**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c6132-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c6132-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c6132-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s cloudem SAP pro zákazníka na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c6132-138">In this section, you configure and test Azure AD single sign-on with SAP Cloud for Customer based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c6132-139">Pro toowork jeden přihlašování Azure AD musí tooknow hello příslušného uživatele v cloudu SAP pro zákazníka je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c6132-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP Cloud for Customer is tooa user in Azure AD.</span></span> <span data-ttu-id="c6132-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v cloudu SAP pro odběratele musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="c6132-140">In other words, a link relationship between an Azure AD user and hello related user in SAP Cloud for Customer needs toobe established.</span></span>

<span data-ttu-id="c6132-141">V cloudu SAP pro zákazníka, přiřadit hodnotu hello hello **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** tooestablish hello odkaz relace.</span><span class="sxs-lookup"><span data-stu-id="c6132-141">In SAP Cloud for Customer, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c6132-142">tooconfigure a testovací Azure AD jednotného přihlášení SAP cloudu pro zákazníka, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c6132-142">tooconfigure and test Azure AD single sign-on with SAP Cloud for Customer, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c6132-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="c6132-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c6132-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c6132-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c6132-145">**[Vytvoření cloudu SAP pro zákazníka testovacího uživatele](#creating-a-sap-cloud-for-customer-test-user)**  -toohave protějšek Britta Simon v cloudu SAP pro zákazníka, která je propojená toohello Azure AD reprezentace uživatele.</span><span class="sxs-lookup"><span data-stu-id="c6132-145">**[Creating a SAP Cloud for Customer test user](#creating-a-sap-cloud-for-customer-test-user)** - toohave a counterpart of Britta Simon in SAP Cloud for Customer that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c6132-146">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c6132-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c6132-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="c6132-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c6132-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c6132-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c6132-149">V této části můžete povolit Azure AD jednotné přihlašování v hello portál Azure a nakonfigurovat jednotné přihlašování ve vašem cloudu SAP pro aplikace pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="c6132-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP Cloud for Customer application.</span></span>

<span data-ttu-id="c6132-150">**tooconfigure Azure AD jednotného přihlášení SAP cloudu pro zákazníka, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c6132-150">**tooconfigure Azure AD single sign-on with SAP Cloud for Customer, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6132-151">V portálu Azure, na hello hello **SAP cloudu pro zákazníka** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c6132-151">In hello Azure portal, on hello **SAP Cloud for Customer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="c6132-153">Na hello **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** tooenable jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c6132-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. <span data-ttu-id="c6132-155">Na hello **SAP cloudu zákazníka domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c6132-155">On hello **SAP Cloud for Customer Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    <span data-ttu-id="c6132-157">a.</span><span class="sxs-lookup"><span data-stu-id="c6132-157">a.</span></span> <span data-ttu-id="c6132-158">V hello **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="c6132-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    <span data-ttu-id="c6132-159">b.</span><span class="sxs-lookup"><span data-stu-id="c6132-159">b.</span></span> <span data-ttu-id="c6132-160">V hello **identifikátor** textovému poli, zadejte adresu URL pomocí hello následující vzoru:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="c6132-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c6132-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="c6132-161">These values are not real.</span></span> <span data-ttu-id="c6132-162">Aktualizovat tyto hodnoty s hello skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="c6132-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c6132-163">Obraťte se na [SAP Cloud pro tým podpory zákazníků klienta](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget tyto hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c6132-163">Contact [SAP Cloud for Customer Client support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget these values.</span></span> 

4. <span data-ttu-id="c6132-164">Na hello **uživatelské atributy** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c6132-164">On hello **User Attributes** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    <span data-ttu-id="c6132-166">a.</span><span class="sxs-lookup"><span data-stu-id="c6132-166">a.</span></span> <span data-ttu-id="c6132-167">V **uživatelský identifikátor** seznamu, vyberte hello **ExtractMailPrefix()** funkce.</span><span class="sxs-lookup"><span data-stu-id="c6132-167">In **User Identifier** list, select hello **ExtractMailPrefix()** function.</span></span>

    <span data-ttu-id="c6132-168">b.</span><span class="sxs-lookup"><span data-stu-id="c6132-168">b.</span></span> <span data-ttu-id="c6132-169">Z hello **e-mailu** seznamu, vyberte hello atribut uživatele chcete toouse týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="c6132-169">From hello **Mail** list, select hello user attribute you want toouse for your implementation.</span></span>
    <span data-ttu-id="c6132-170">Například pokud chcete, aby toouse hello EmployeeID jako jedinečný identifikátor uživatele a hodnota atributu hello jsou uloženy v hello ExtensionAttribute2, pak vyberte user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="c6132-170">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select user.extensionattribute2.</span></span>  

5. <span data-ttu-id="c6132-171">Na hello **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c6132-171">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. <span data-ttu-id="c6132-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6132-173">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="c6132-175">Na hello **SAP cloudu pro konfiguraci zákazníka** klikněte na tlačítko **konfigurace cloudu SAP pro zákazníka** tooopen **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="c6132-175">On hello **SAP Cloud for Customer Configuration** section, click **Configure SAP Cloud for Customer** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="c6132-176">Kopírování hello **SAML jeden přihlašování adresa URL služby** z hello **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="c6132-176">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. <span data-ttu-id="c6132-178">tooget jednotného přihlašování nakonfigurovaná, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c6132-178">tooget SSO configured, perform hello following steps:</span></span>
   
    <span data-ttu-id="c6132-179">a.</span><span class="sxs-lookup"><span data-stu-id="c6132-179">a.</span></span> <span data-ttu-id="c6132-180">Přihlášení do cloudu SAP pro zákaznický portál s právy správce.</span><span class="sxs-lookup"><span data-stu-id="c6132-180">Login into SAP Cloud for Customer portal with administrator rights.</span></span>
   
    <span data-ttu-id="c6132-181">b.</span><span class="sxs-lookup"><span data-stu-id="c6132-181">b.</span></span> <span data-ttu-id="c6132-182">Přejděte toohello **aplikace a běžné úlohy správy uživatele** a klikněte na tlačítko hello **zprostředkovatele Identity** kartě.</span><span class="sxs-lookup"><span data-stu-id="c6132-182">Navigate toohello **Application and User Management Common Task** and click hello **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="c6132-183">c.</span><span class="sxs-lookup"><span data-stu-id="c6132-183">c.</span></span> <span data-ttu-id="c6132-184">Klikněte na tlačítko **nového poskytovatele Identity** a soubor XML metadat vyberte hello jste si stáhli z portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="c6132-184">Click **New Identity Provider** and select hello metadata XML file you have downloaded from hello Azure portal.</span></span> <span data-ttu-id="c6132-185">Importováním hello metadata hello systému automaticky nahrává hello dodejka certifikát a certifikát pro šifrování.</span><span class="sxs-lookup"><span data-stu-id="c6132-185">By importing hello metadata, hello system automatically uploads hello required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    <span data-ttu-id="c6132-187">d.</span><span class="sxs-lookup"><span data-stu-id="c6132-187">d.</span></span> <span data-ttu-id="c6132-188">Azure Active Directory vyžaduje hello element adresa URL služby Assertion příjemce v žádost SAML hello, takže vyberte hello **zahrnují Assertion příjemce adresa URL služby** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="c6132-188">Azure Active Directory requires hello element Assertion Consumer Service URL in hello SAML request, so select hello **Include Assertion Consumer Service URL** checkbox.</span></span>
   
    <span data-ttu-id="c6132-189">e.</span><span class="sxs-lookup"><span data-stu-id="c6132-189">e.</span></span> <span data-ttu-id="c6132-190">Klikněte na tlačítko **aktivovat jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c6132-190">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="c6132-191">f.</span><span class="sxs-lookup"><span data-stu-id="c6132-191">f.</span></span> <span data-ttu-id="c6132-192">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="c6132-192">Save your changes.</span></span>
   
    <span data-ttu-id="c6132-193">g.</span><span class="sxs-lookup"><span data-stu-id="c6132-193">g.</span></span> <span data-ttu-id="c6132-194">Klikněte na tlačítko hello **Moje systému** kartě.</span><span class="sxs-lookup"><span data-stu-id="c6132-194">Click hello **My System** tab.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    <span data-ttu-id="c6132-196">h.</span><span class="sxs-lookup"><span data-stu-id="c6132-196">h.</span></span> <span data-ttu-id="c6132-197">V **Azure AD adresa URL přihlašování** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c6132-197">In **Azure AD Sign On URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    <span data-ttu-id="c6132-199">i.</span><span class="sxs-lookup"><span data-stu-id="c6132-199">i.</span></span> <span data-ttu-id="c6132-200">Zadejte, zda zaměstnanec hello ručně vybrat mezi přihlásit pomocí ID uživatele a heslo nebo jednotného přihlašování k výběrem hello **výběr zprostředkovatele Identity ruční**.</span><span class="sxs-lookup"><span data-stu-id="c6132-200">Specify whether hello employee can manually choose between logging on with user ID and password or SSO by selecting hello **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="c6132-201">j.</span><span class="sxs-lookup"><span data-stu-id="c6132-201">j.</span></span> <span data-ttu-id="c6132-202">V hello **jednotného přihlašování k adrese URL** části, zadejte adresu URL hello, který má být používána vaši zaměstnanci toosign systému toohello.</span><span class="sxs-lookup"><span data-stu-id="c6132-202">In hello **SSO URL** section, specify hello URL that should be used by your employees toosign on toohello system.</span></span> 
    <span data-ttu-id="c6132-203">V hello **odeslané na adresu URL tooEmployee** seznamu, můžete si vybrat mezi hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="c6132-203">In hello **URL Sent tooEmployee** list, you can choose between hello following options:</span></span>
   
    <span data-ttu-id="c6132-204">**Adresa URL jednotného přihlašování**</span><span class="sxs-lookup"><span data-stu-id="c6132-204">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="c6132-205">systém Hello odešle jenom hello normální systému URL toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="c6132-205">hello system sends only hello normal system URL toohello employee.</span></span> <span data-ttu-id="c6132-206">Zaměstnanec Hello nelze přihlášení pomocí jednotného přihlašování a musí používat heslo nebo místo toho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="c6132-206">hello employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="c6132-207">**ADRESA URL JEDNOTNÉHO PŘIHLAŠOVÁNÍ**</span><span class="sxs-lookup"><span data-stu-id="c6132-207">**SSO URL**</span></span> 
   
    <span data-ttu-id="c6132-208">systém Hello odešle jenom hello zaměstnanec toohello jednotného přihlašování k adrese URL.</span><span class="sxs-lookup"><span data-stu-id="c6132-208">hello system sends only hello SSO URL toohello employee.</span></span> <span data-ttu-id="c6132-209">Hello zaměstnanců může přihlásit pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c6132-209">hello employee can log on using SSO.</span></span> <span data-ttu-id="c6132-210">Prostřednictvím hello IdP přesměruje požadavek na ověření.</span><span class="sxs-lookup"><span data-stu-id="c6132-210">Authentication request is redirected through hello IdP.</span></span>
   
    <span data-ttu-id="c6132-211">**Automatický výběr**</span><span class="sxs-lookup"><span data-stu-id="c6132-211">**Automatic Selection**</span></span>
   
    <span data-ttu-id="c6132-212">Pokud jednotného přihlašování není aktivní, odešle hello systému hello normální systému URL toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="c6132-212">If SSO is not active, hello system sends hello normal system URL toohello employee.</span></span> <span data-ttu-id="c6132-213">Pokud je aktivní jednotné přihlašování, hello systému zkontroluje, zda hello zaměstnanec má heslo.</span><span class="sxs-lookup"><span data-stu-id="c6132-213">If SSO is active, hello system checks whether hello employee has a password.</span></span> <span data-ttu-id="c6132-214">Pokud heslo je k dispozici, jednotného přihlašování k adrese URL a adresy URL bez jednotného přihlašování k odešlou toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="c6132-214">If a password is available, both SSO URL and Non-SSO URL are sent toohello employee.</span></span> <span data-ttu-id="c6132-215">Ale pokud zaměstnanec hello nemá žádné heslo, pouze hello URL jednotného přihlašování k odešle toohello zaměstnanců.</span><span class="sxs-lookup"><span data-stu-id="c6132-215">However, if hello employee has no password, only hello SSO URL is sent toohello employee.</span></span>
   
    <span data-ttu-id="c6132-216">kB.</span><span class="sxs-lookup"><span data-stu-id="c6132-216">k.</span></span> <span data-ttu-id="c6132-217">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="c6132-217">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="c6132-218">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř hello [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace hello!</span><span class="sxs-lookup"><span data-stu-id="c6132-218">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c6132-219">Po přidání této aplikace z hello **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na tlačítko hello **jednotné přihlašování** kartě a přístup hello vložených dokumentace prostřednictvím hello  **Konfigurace** části dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="c6132-219">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c6132-220">Si můžete přečíst více o hello embedded dokumentace funkci zde: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c6132-220">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c6132-221">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c6132-221">Creating an Azure AD test user</span></span>
<span data-ttu-id="c6132-222">Hello cílem této části je toocreate testovacího uživatele v portálu Azure, názvem Britta Simon hello.</span><span class="sxs-lookup"><span data-stu-id="c6132-222">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="c6132-224">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c6132-224">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6132-225">V hello **portál Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c6132-225">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c6132-227">toodisplay hello seznam uživatelů, přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c6132-227">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c6132-229">tooopen hello **uživatele** dialogové okno, klikněte na tlačítko **přidat** hello nahoře hello dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="c6132-229">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c6132-231">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c6132-231">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c6132-233">a.</span><span class="sxs-lookup"><span data-stu-id="c6132-233">a.</span></span> <span data-ttu-id="c6132-234">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c6132-234">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c6132-235">b.</span><span class="sxs-lookup"><span data-stu-id="c6132-235">b.</span></span> <span data-ttu-id="c6132-236">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c6132-236">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c6132-237">c.</span><span class="sxs-lookup"><span data-stu-id="c6132-237">c.</span></span> <span data-ttu-id="c6132-238">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="c6132-238">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c6132-239">d.</span><span class="sxs-lookup"><span data-stu-id="c6132-239">d.</span></span> <span data-ttu-id="c6132-240">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c6132-240">Click **Create**.</span></span>
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a><span data-ttu-id="c6132-241">Vytvoření cloudu SAP pro zákazníka testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c6132-241">Creating a SAP Cloud for Customer test user</span></span>

<span data-ttu-id="c6132-242">V této části vytvoříte uživatele volat Britta Simon v cloudu SAP pro zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c6132-242">In this section, you create a user called Britta Simon in SAP Cloud for Customer.</span></span> <span data-ttu-id="c6132-243">Spojte se s [SAP Cloud pro tým podpory zákazníků](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello uživatele v hello SAP cloudu pro platformu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c6132-243">Please work with [SAP Cloud for Customer support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello users in hello SAP Cloud for Customer platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="c6132-244">Zkontrolujte, zda hodnota NameID by měl odpovídat hello pole pro uživatelské jméno v hello SAP cloudu pro platformu zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c6132-244">Please make sure that NameID value should match with hello username field in hello SAP Cloud for Customer platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c6132-245">Přiřazení hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="c6132-245">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c6132-246">V této části je povolit Britta Simon toouse Azure jednotné přihlašování tak, že udělíte přístup tooSAP cloudu pro zákazníka.</span><span class="sxs-lookup"><span data-stu-id="c6132-246">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP Cloud for Customer.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="c6132-248">**tooassign tooSAP Britta Simon cloudu pro zákazníka, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="c6132-248">**tooassign Britta Simon tooSAP Cloud for Customer, perform hello following steps:**</span></span>

1. <span data-ttu-id="c6132-249">V hello portálu Azure, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c6132-249">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c6132-251">V seznamu aplikace hello vyberte **SAP cloudu pro zákazníka**.</span><span class="sxs-lookup"><span data-stu-id="c6132-251">In hello applications list, select **SAP Cloud for Customer**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. <span data-ttu-id="c6132-253">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c6132-253">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="c6132-255">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c6132-255">Click **Add** button.</span></span> <span data-ttu-id="c6132-256">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c6132-256">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="c6132-258">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="c6132-258">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c6132-259">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c6132-259">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c6132-260">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c6132-260">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c6132-261">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c6132-261">Testing single sign-on</span></span>

<span data-ttu-id="c6132-262">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="c6132-262">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c6132-263">Po kliknutí na tlačítko hello SAP cloudu pro dlaždice zákazníka v hello přístupového panelu, měli byste obdržet tooyour automaticky přihlášeného SAP cloudu pro aplikace pro zákazníky.</span><span class="sxs-lookup"><span data-stu-id="c6132-263">When you click hello SAP Cloud for Customer tile in hello Access Panel, you should get automatically signed-on tooyour SAP Cloud for Customer application.</span></span>
<span data-ttu-id="c6132-264">Další informace o hello přístupového panelu najdete v tématu [toohello Úvod přístupový Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c6132-264">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6132-265">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c6132-265">Additional resources</span></span>

* [<span data-ttu-id="c6132-266">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c6132-266">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c6132-267">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c6132-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

