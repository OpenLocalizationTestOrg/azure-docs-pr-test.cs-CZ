---
title: "Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování | Microsoft Docs"
description: "Zjistěte, jak tooconfigure jednotné přihlašování mezi Azure Active Directory a SAP HANA cloudové platformy identitu ověřování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2142770779ddb745555b47fc85b5457b573f9506
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a><span data-ttu-id="9f533-103">Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování</span><span class="sxs-lookup"><span data-stu-id="9f533-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform Identity Authentication</span></span>

<span data-ttu-id="9f533-104">V tomto kurzu zjistíte, jak toointegrate SAP HANA cloudové platformy identitu ověřování s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9f533-104">In this tutorial, you learn how toointegrate SAP HANA Cloud Platform Identity Authentication with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="9f533-105">Ověření Identity SAP HANA cloudové platformy slouží jako proxy IdP tooaccess SAP aplikací pomocí služby Azure AD jako hlavní IdP hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-105">SAP HANA Cloud Platform Identity Authentication is used as a proxy IdP tooaccess SAP applications using Azure AD as hello main IdP.</span></span>

<span data-ttu-id="9f533-106">Integrace SAP HANA cloudové platformy identitu ověřování s Azure AD poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9f533-106">Integrating SAP HANA Cloud Platform Identity Authentication with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9f533-107">Můžete řídit ve službě Azure AD, který má přístup tooSAP aplikace</span><span class="sxs-lookup"><span data-stu-id="9f533-107">You can control in Azure AD who has access tooSAP application</span></span>
- <span data-ttu-id="9f533-108">Můžete povolit vaši uživatelé tooautomatically get přihlášeného tooSAP aplikace jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f533-108">You can enable your users tooautomatically get signed-on tooSAP applications single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="9f533-109">Můžete spravovat vaše účty v jednom centrálním místě - hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="9f533-109">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="9f533-110">Pokud chcete tooknow Další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f533-110">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9f533-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9f533-111">Prerequisites</span></span>

<span data-ttu-id="9f533-112">tooconfigure integrace Azure AD s SAP HANA cloudové platformy identitu ověřování, je třeba hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="9f533-112">tooconfigure Azure AD integration with SAP HANA Cloud Platform Identity Authentication, you need hello following items:</span></span>

- <span data-ttu-id="9f533-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f533-113">An Azure AD subscription</span></span>
- <span data-ttu-id="9f533-114">A **SAP HANA cloudové platformy identitu ověřování** předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f533-114">A **SAP HANA Cloud Platform Identity Authentication** SSO enabled subscription</span></span>


>[!NOTE] 
><span data-ttu-id="9f533-115">tootest hello kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f533-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="9f533-116">tootest hello kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9f533-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9f533-117">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9f533-117">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9f533-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f533-118">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f533-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9f533-119">Scenario description</span></span>
<span data-ttu-id="9f533-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9f533-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="9f533-121">Hello scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9f533-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9f533-122">Přidání ověření Identity SAP HANA cloudové platformy z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9f533-122">Adding SAP HANA Cloud Platform Identity Authentication from hello gallery</span></span>
2. <span data-ttu-id="9f533-123">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="9f533-123">Configuring and testing Azure AD SSO</span></span>

<span data-ttu-id="9f533-124">Než začnete hello technické podrobnosti, je důležité toounderstand hello koncepty, které budete toolook v.</span><span class="sxs-lookup"><span data-stu-id="9f533-124">Before diving into hello technical details, it is vital toounderstand hello concepts you're going toolook at.</span></span> <span data-ttu-id="9f533-125">Hello SAP HANA cloudové platformy identitu ověřování a Azure Active Directory federation vám umožní tooimplement jednotného přihlašování napříč aplikacím nebo službám chráněným pomocí AAD (jako IdP) s aplikacemi SAP a službám chráněným pomocí SAP HANA Cloudová platforma identita Ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f533-125">hello SAP HANA Cloud Platform Identity Authentication and Azure Active Directory federation enables you tooimplement SSO across applications or services protected by AAD (as an IdP) with SAP applications and services protected by SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="9f533-126">V současné době SAP HANA cloudové platformy identitu ověřování funguje jako zprostředkovatel Identity Proxy tooSAP – aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f533-126">Currently, SAP HANA Cloud Platform Identity Authentication acts as a Proxy Identity Provider tooSAP-applications.</span></span> <span data-ttu-id="9f533-127">Azure Active Directory pak funguje jako hello úvodní zprostředkovatele Identity v rámci této instalace.</span><span class="sxs-lookup"><span data-stu-id="9f533-127">Azure Active Directory in turn acts as hello leading Identity Provider in this setup.</span></span> 

<span data-ttu-id="9f533-128">Hello následující diagram znázorňuje toto:</span><span class="sxs-lookup"><span data-stu-id="9f533-128">hello following diagram illustrates this:</span></span>    

![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

<span data-ttu-id="9f533-130">S tímto nastavením budou klientovi SAP HANA cloudové platformy identitu ověřování nakonfigurované jako důvěryhodné aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9f533-130">With this setup, your SAP HANA Cloud Platform Identity Authentication tenant will be configured as a trusted application in Azure Active Directory.</span></span> 

<span data-ttu-id="9f533-131">Všechny aplikace SAP a služby, které mají tooprotect prostřednictvím tímto způsobem jsou následně nakonfigurované v konzole pro správu SAP HANA cloudové platformy identitu ověřování hello!</span><span class="sxs-lookup"><span data-stu-id="9f533-131">All SAP applications and services you want tooprotect through this way are subsequently configured in hello SAP HANA Cloud Platform Identity Authentication management console!</span></span>

<span data-ttu-id="9f533-132">To znamená, že autorizace pro udělení přístupu tooSAP aplikace a služby potřebám tootake místní v SAP HANA cloudové platformy identitu ověřování pro taková konfigurace (jako názvem na rozdíl od tooconfiguring autorizace v Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="9f533-132">This means that authorization for granting access tooSAP applications and services needs tootake place in SAP HANA Cloud Platform Identity Authentication for such a setup (as opposed tooconfiguring authorization in Azure Active Directory).</span></span>

<span data-ttu-id="9f533-133">Nakonfigurováním ověření Identity SAP HANA cloudové platformy jako aplikaci prostřednictvím hello Azure Active Directory Marketplace nepotřebujete tootake péče o konfiguraci potřebné jednotlivé deklaracích / SAML kontrolní výrazy a transformace potřeby tooproduce platné ověřovací token pro aplikace SAP.</span><span class="sxs-lookup"><span data-stu-id="9f533-133">By configuring SAP HANA Cloud Platform Identity Authentication as an application through hello Azure Active Directory Marketplace, you don't need tootake care of configuring needed individual claims / SAML assertions and transformations needed tooproduce a valid authentication token for SAP applications.</span></span>

>[!NOTE] 
><span data-ttu-id="9f533-134">Obě strany, pouze byl testován aktuálně jednotného přihlašování k webu.</span><span class="sxs-lookup"><span data-stu-id="9f533-134">Currently Web SSO has been tested by both parties, only.</span></span> <span data-ttu-id="9f533-135">Toky potřebné pro aplikaci API nebo rozhraní API API komunikace by měla fungovat, ale nebyly testovány, ještě.</span><span class="sxs-lookup"><span data-stu-id="9f533-135">Flows needed for App-to-API or API-to-API communication should work but have not been tested, yet.</span></span> <span data-ttu-id="9f533-136">Bude být testována jako součást následné aktivity.</span><span class="sxs-lookup"><span data-stu-id="9f533-136">They will be tested as part of subsequent activities.</span></span>
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-hello-gallery"></a><span data-ttu-id="9f533-137">Přidání ověření Identity SAP HANA cloudové platformy z Galerie hello</span><span class="sxs-lookup"><span data-stu-id="9f533-137">Add SAP HANA Cloud Platform Identity Authentication from hello gallery</span></span>
<span data-ttu-id="9f533-138">tooconfigure hello integrace SAP HANA cloudové platformy identitu ověřování do služby Azure AD, je nutné tooadd SAP HANA cloudové platformy identitu ověřování hello Galerie tooyour seznamu spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9f533-138">tooconfigure hello integration of SAP HANA Cloud Platform Identity Authentication into Azure AD, you need tooadd SAP HANA Cloud Platform Identity Authentication from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9f533-139">**tooadd SAP HANA cloudové platformy identitu ověřování z Galerie hello, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f533-139">**tooadd SAP HANA Cloud Platform Identity Authentication from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f533-140">V hello [ **portálu pro správu Azure**](https://portal.azure.com), na levém navigačním panelu text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9f533-140">In hello [**Azure Management portal**](https://portal.azure.com), on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9f533-142">Přejděte příliš**podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9f533-142">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9f533-143">Potom přejděte příliš**všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f533-143">Then go too**All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9f533-145">Klikněte na tlačítko **přidat** hello nahoře hello dialogového okna na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f533-145">Click **Add** button on hello top of hello dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9f533-147">Hello vyhledávacího pole zadejte **SAP HANA cloudové platformy identitu ověřování**.</span><span class="sxs-lookup"><span data-stu-id="9f533-147">In hello search box, type **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. <span data-ttu-id="9f533-149">Na panelu výsledků hello vyberte **SAP HANA cloudové platformy identitu ověřování**a potom klikněte na **přidat** tlačítko tooadd hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f533-149">In hello results panel, select **SAP HANA Cloud Platform Identity Authentication**, and then click **Add** button tooadd hello application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9f533-151">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f533-151">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9f533-152">V této části konfiguraci a testování Azure AD přihlášení SSO se SAP HANA cloudové platformy Identity ověřování na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="9f533-152">In this section, you configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9f533-153">Pro jednotné přihlašování toowork Azure AD musí tooknow, jaké hello příslušného uživatele v SAP HANA cloudové platformy identitu ověřování je tooa uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f533-153">For SSO toowork, Azure AD needs tooknow what hello counterpart user in SAP HANA Cloud Platform Identity Authentication is tooa user in Azure AD.</span></span> <span data-ttu-id="9f533-154">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské hello v SAP HANA cloudové platformy identitu ověřování musí toobe navázat.</span><span class="sxs-lookup"><span data-stu-id="9f533-154">In other words, a link relationship between an Azure AD user and hello related user in SAP HANA Cloud Platform Identity Authentication needs toobe established.</span></span>

<span data-ttu-id="9f533-155">Přiřazením hello hodnotu hello je vytvořen vztah tento odkaz **uživatelské jméno** ve službě Azure AD jako hodnota hello hello **uživatelské jméno** v SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f533-155">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="9f533-156">tooconfigure a testování Azure AD přihlášení SSO se SAP HANA cloudové platformy identitu ověřování, potřebujete následující stavební bloky hello toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9f533-156">tooconfigure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9f533-157">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  -tooenable toouse vaši uživatelé tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="9f533-157">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9f533-158">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  -tootest Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f533-158">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9f533-159">**[Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  -toohave protějšek Britta Simon v SAP HANA cloudové platformy Identity ověřování, která je jí reprezentace toohello propojené služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f533-159">**[Creating a SAP HANA Cloud Platform Identity Authentication test user](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** - toohave a counterpart of Britta Simon in SAP HANA Cloud Platform Identity Authentication that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9f533-160">**[Přiřazení hello Azure AD testovacího uživatele](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9f533-160">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9f533-161">**[Testování jednotné přihlašování](#testing-single-sign-on)**  -tooverify tom, zda text hello konfigurace funguje.</span><span class="sxs-lookup"><span data-stu-id="9f533-161">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="9f533-162">Konfigurace Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f533-162">Configuring Azure AD SSO</span></span>

<span data-ttu-id="9f533-163">V této části můžete povolit jednotné přihlašování Azure AD na portálu pro správu Azure hello a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f533-163">In this section, you enable Azure AD SSO in hello Azure Management portal and configure single sign-on in your SAP HANA Cloud Platform Identity Authentication application.</span></span>

<span data-ttu-id="9f533-164">Ověření Identity SAP HANA cloudové platformy aplikace očekává hello SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="9f533-164">SAP HANA Cloud Platform Identity Authentication application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="9f533-165">Můžete spravovat hello hodnoty těchto atributů z hello "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f533-165">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="9f533-166">Hello následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="9f533-166">hello following screenshot shows an example for this.</span></span>

![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

<span data-ttu-id="9f533-168">**tooconfigure Azure AD přihlášení SSO se SAP HANA cloudové platformy identitu ověřování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f533-168">**tooconfigure Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f533-169">V hello Azure Management portal na hello **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9f533-169">In hello Azure Management portal, on hello **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9f533-171">Na hello **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** jednotného přihlašování k tooenable.</span><span class="sxs-lookup"><span data-stu-id="9f533-171">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování][5]

3. <span data-ttu-id="9f533-173">V hello **uživatelské atributy** část hello **jednotného přihlašování** dialogové okno, pokud vaše aplikace SAP očekává atribut například "jméno".</span><span class="sxs-lookup"><span data-stu-id="9f533-173">In hello **User Attributes** section on hello **Single sign-on** dialog, if your SAP application expects an attribute for example "firstName".</span></span> <span data-ttu-id="9f533-174">V dialogovém okně atributy tokenu SAML hello přidejte atribut hello "jméno".</span><span class="sxs-lookup"><span data-stu-id="9f533-174">On hello SAML token attributes dialog, add hello "firstName" attribute.</span></span>
 1. <span data-ttu-id="9f533-175">Klikněte na tlačítko **přidat atribut** tooopen hello **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f533-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
 
    ![Konfigurovat jednotné přihlašování][6]

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. <span data-ttu-id="9f533-178">V hello **název atributu** textovému poli, zadejte název atributu hello "jméno".</span><span class="sxs-lookup"><span data-stu-id="9f533-178">In hello **Attribute Name** textbox, type hello attribute name "firstName".</span></span>
 3. <span data-ttu-id="9f533-179">Z hello **hodnota atributu** seznamu, vyberte hello hodnotu atributu "user.givenname".</span><span class="sxs-lookup"><span data-stu-id="9f533-179">From hello **Attribute Value** list, select hello attribute value "user.givenname".</span></span>
 4. <span data-ttu-id="9f533-180">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9f533-180">Click **Ok**.</span></span>

4. <span data-ttu-id="9f533-181">Na hello **SAP HANA cloudové platformy identitu ověřování domény a adresy URL** část, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="9f533-181">On hello **SAP HANA Cloud Platform Identity Authentication Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. <span data-ttu-id="9f533-183">V hello **přihlašovací adresa URL** textovému poli, zadejte přihlašovací hello na adresu URL pro aplikaci SAP hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-183">In hello **Sign On URL** textbox, type hello sign on URL for hello SAP application.</span></span>
 2. <span data-ttu-id="9f533-184">V hello **identifikátor** textovému poli, hodnota typu hello následující vzoru:`<entity-id>.accounts.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="9f533-184">In hello **Identifier** textbox, type hello value following pattern: `<entity-id>.accounts.ondemand.com`</span></span> 
    * <span data-ttu-id="9f533-185">Pokud si nejste jisti tuto hodnotu, postupujte podle dokumentace hello SAP HANA cloudové platformy identitu ověřování v [konfiguraci klienta SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span><span class="sxs-lookup"><span data-stu-id="9f533-185">If you don't know this value, please follow hello SAP HANA Cloud Platform Identity Authentication documentation on [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span></span>

5. <span data-ttu-id="9f533-186">Na hello **konfiguraci Identity ověřování cloudové platformy SAP HANA** klikněte na tlačítko **konfigurace SAP HANA cloudové platformy identitu ověřování** tooopen **konfigurovatpřihlášení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f533-186">On hello **SAP HANA Cloud Platform Identity Authentication Configuration** section, click **Configure SAP HANA Cloud Platform Identity Authentication** tooopen **Configure sign-on** dialog.</span></span> <span data-ttu-id="9f533-187">Potom klikněte na **SAML XML Metadata** a uložte soubor hello ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="9f533-187">Then, click on **SAML XML Metadata** and save hello file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. <span data-ttu-id="9f533-190">tooget jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte tooSAP HANA cloudové platformy identitu ověřování správy konzoly.</span><span class="sxs-lookup"><span data-stu-id="9f533-190">tooget SSO configured for your application, go tooSAP HANA Cloud Platform Identity Authentication Administration Console.</span></span> <span data-ttu-id="9f533-191">Adresa URL Hello má následující vzor hello:`https://<tenant-id>.accounts.ondemand.com/admin`</span><span class="sxs-lookup"><span data-stu-id="9f533-191">hello URL has hello following pattern: `https://<tenant-id>.accounts.ondemand.com/admin`</span></span>
 * <span data-ttu-id="9f533-192">Postupujte podle dokumentace hello na ověření Identity SAP HANA cloudové platformy příliš[konfigurace Microsoft Azure AD jako podnikové poskytovatele Identity v SAP HANA cloudové platformy identitu ověřování](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span><span class="sxs-lookup"><span data-stu-id="9f533-192">Then, follow hello documentation on SAP HANA Cloud Platform Identity Authentication too[Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span></span> 

7. <span data-ttu-id="9f533-193">Na portálu pro správu Azure hello, klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f533-193">In hello Azure Management portal, click **Save** button.</span></span>
8. <span data-ttu-id="9f533-194">Pokračujte hello pouze v případě, že chcete tooadd a povolení jednotného přihlašování pro jinou aplikaci SAP následující kroky.</span><span class="sxs-lookup"><span data-stu-id="9f533-194">Continue hello following steps only if you want tooadd and enable SSO for another SAP application.</span></span> <span data-ttu-id="9f533-195">Opakujte kroky v části hello části "Přidání SAP HANA cloudové platformy identitu ověřování z Galerie hello" tooadd jiná instance SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f533-195">Repeat steps under hello section “Adding SAP HANA Cloud Platform Identity Authentication from hello gallery” tooadd another instance of SAP HANA Cloud Platform Identity Authentication.</span></span>
9. <span data-ttu-id="9f533-196">V hello Azure Management portal na hello **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **propojené přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="9f533-196">In hello Azure Management portal, on hello **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Linked Sign-on**.</span></span>

    ![Konfigurovat propojené přihlášení](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. <span data-ttu-id="9f533-198">Potom uložte konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-198">Then, save hello configuration.</span></span>

>[!NOTE] 
><span data-ttu-id="9f533-199">Nová aplikace Hello využije hello Konfigurace jednotného přihlašování pro předchozí aplikaci SAP hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-199">hello new application will leverage hello SSO configuration for hello previous SAP application.</span></span> <span data-ttu-id="9f533-200">Zkontrolujte, zda je použití hello stejné podnikové poskytovatelů identit v hello SAP HANA cloudové platformy identitu ověřování konzole pro správu.</span><span class="sxs-lookup"><span data-stu-id="9f533-200">Please make sure you use hello same Corporate Identity Providers in hello SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span>
>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9f533-201">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f533-201">Create an Azure AD test user</span></span>
<span data-ttu-id="9f533-202">Hello cílem této části je toocreate testovacího uživatele v hello názvem Britta Simon nového portálu.</span><span class="sxs-lookup"><span data-stu-id="9f533-202">hello objective of this section is toocreate a test user in hello new portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9f533-204">**toocreate testovacího uživatele ve službě Azure AD, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f533-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f533-205">V hello **portálu pro správu Azure**, na levém navigačním podokně text hello, klikněte na **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9f533-205">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9f533-207">Přejděte příliš**uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** toodisplay hello seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9f533-207">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9f533-209">V horní části hello hello dialogového okna klikněte na tlačítko **přidat** tooopen hello **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f533-209">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9f533-211">Na hello **uživatele** dialogové okno proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9f533-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. <span data-ttu-id="9f533-213">V hello **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f533-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>
  2. <span data-ttu-id="9f533-214">V hello **uživatelské jméno** textovému poli, typ hello **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9f533-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>
  3. <span data-ttu-id="9f533-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu hello hello **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9f533-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>
  4. <span data-ttu-id="9f533-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9f533-216">Click **Create**.</span></span> 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a><span data-ttu-id="9f533-217">Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování</span><span class="sxs-lookup"><span data-stu-id="9f533-217">Create a SAP HANA Cloud Platform Identity Authentication test user</span></span>

<span data-ttu-id="9f533-218">Nepotřebujete toocreate uživatelé na SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f533-218">You don't need toocreate an user on SAP HANA Cloud Platform Identity Authentication.</span></span> <span data-ttu-id="9f533-219">Uživatelé, kteří jsou v úložišti uživatele hello Azure AD můžete použít funkci jednotného přihlašování k hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-219">Users who are in hello Azure AD user store can use hello SSO functionality.</span></span>

<span data-ttu-id="9f533-220">Ověření Identity SAP HANA cloudové platformy podporuje možnost hello federaci identit.</span><span class="sxs-lookup"><span data-stu-id="9f533-220">SAP HANA Cloud Platform Identity Authentication supports hello Identity Federation option.</span></span> <span data-ttu-id="9f533-221">Tato možnost umožňuje toocheck aplikace hello, pokud existují uživatelé hello ověřována hello podnikové identitě zprostředkovatele v hello úložiště systému SAP HANA cloudové platformy identitu ověřování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9f533-221">This option allows hello application toocheck if hello users authenticated by hello corporate identity provider exist in hello user store of SAP HANA Cloud Platform Identity Authentication.</span></span> 

<span data-ttu-id="9f533-222">Ve výchozím nastavení je hello je zakázána hello možnost federaci identit.</span><span class="sxs-lookup"><span data-stu-id="9f533-222">In hello default setting, hello Identity Federation option is disabled.</span></span> <span data-ttu-id="9f533-223">Pokud je povoleno federaci identit, jsou pouze hello uživatelé, kteří jsou importovány v SAP HANA cloudové platformy identitu ověřování aplikací mít tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-223">If Identity Federation is enabled, only hello users that are imported in SAP HANA Cloud Platform Identity Authentication are able tooaccess hello application.</span></span> 

<span data-ttu-id="9f533-224">Další informace o tom, jak tooenable nebo zakázat federaci identit s SAP HANA cloudové platformy identitu ověřování, najdete v části Povolit federaci identit s SAP HANA cloudové platformy identitu ověřování v [konfigurace federaci identit s hello úložiště systému SAP HANA cloudové platformy identitu ověřování uživatelů. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span><span class="sxs-lookup"><span data-stu-id="9f533-224">For more information about how tooenable or disable Identity Federation with SAP HANA Cloud Platform Identity Authentication, see Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication in [Configure Identity Federation with hello User Store of SAP HANA Cloud Platform Identity Authentication.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9f533-225">Přiřadit hello Azure AD testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9f533-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9f533-226">V této části povolíte Britta Simon toouse Azure jednotné přihlašování, poskytněte svůj přístup tooSAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="9f533-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooSAP HANA Cloud Platform Identity Authentication.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9f533-228">**tooassign Britta Simon tooSAP HANA cloudové platformy identitu ověřování, proveďte následující kroky hello:**</span><span class="sxs-lookup"><span data-stu-id="9f533-228">**tooassign Britta Simon tooSAP HANA Cloud Platform Identity Authentication, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f533-229">Na portálu pro správu Azure hello, otevřete zobrazení aplikace hello a potom přejděte toohello directory zobrazení a přejděte příliš**podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9f533-229">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9f533-231">V seznamu aplikace hello vyberte **SAP HANA cloudové platformy identitu ověřování**.</span><span class="sxs-lookup"><span data-stu-id="9f533-231">In hello applications list, select **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. <span data-ttu-id="9f533-233">V nabídce hello hello vlevo, klikněte na **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9f533-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9f533-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9f533-235">Click **Add** button.</span></span> <span data-ttu-id="9f533-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f533-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9f533-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelé hello.</span><span class="sxs-lookup"><span data-stu-id="9f533-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9f533-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f533-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9f533-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9f533-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="test-single-sign-on"></a><span data-ttu-id="9f533-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9f533-241">Test single sign-on</span></span>

<span data-ttu-id="9f533-242">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování pomocí hello přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9f533-242">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="9f533-243">Když kliknete na dlaždici hello SAP HANA cloudové platformy identitu ověřování v hello přístupového panelu, měli byste obdržet automaticky přihlášeného tooyour SAP HANA cloudové platformy identitu ověřování aplikace.</span><span class="sxs-lookup"><span data-stu-id="9f533-243">When you click hello SAP HANA Cloud Platform Identity Authentication tile in hello Access Panel, you should get automatically signed-on tooyour SAP HANA Cloud Platform Identity Authentication application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9f533-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9f533-244">Additional resources</span></span>

* [<span data-ttu-id="9f533-245">Seznam kurzů tooIntegrate SaaS aplikací s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f533-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f533-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f533-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png