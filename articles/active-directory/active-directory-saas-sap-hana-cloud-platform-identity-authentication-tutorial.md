---
title: "Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP HANA cloudové platformy identitu ověřování."
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
ms.openlocfilehash: 7799bf03cc6705f805a48f329a265a3d84bed55f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a><span data-ttu-id="97ef1-103">Kurz: Azure Active Directory integrace s SAP HANA cloudové platformy identitu ověřování</span><span class="sxs-lookup"><span data-stu-id="97ef1-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform Identity Authentication</span></span>

<span data-ttu-id="97ef1-104">V tomto kurzu zjistěte, jak integrovat SAP HANA cloudové platformy identitu ověřování s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="97ef1-104">In this tutorial, you learn how to integrate SAP HANA Cloud Platform Identity Authentication with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="97ef1-105">Ověření Identity SAP HANA cloudové platformy slouží jako proxy server poskytovatelů identity pro přístup k SAP aplikacím pomocí služby Azure AD jako hlavní deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="97ef1-105">SAP HANA Cloud Platform Identity Authentication is used as a proxy IdP to access SAP applications using Azure AD as the main IdP.</span></span>

<span data-ttu-id="97ef1-106">Integrace SAP HANA cloudové platformy identitu ověřování s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="97ef1-106">Integrating SAP HANA Cloud Platform Identity Authentication with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="97ef1-107">Můžete řídit ve službě Azure AD, kdo má přístup k aplikaci SAP</span><span class="sxs-lookup"><span data-stu-id="97ef1-107">You can control in Azure AD who has access to SAP application</span></span>
- <span data-ttu-id="97ef1-108">Můžete povolit uživatelům, aby automaticky získat přihlášení k SAP aplikace jednotné přihlašování (SSO) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="97ef1-108">You can enable your users to automatically get signed-on to SAP applications single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="97ef1-109">Můžete spravovat vaše účty v jednom centrálním místě – portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="97ef1-109">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="97ef1-110">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="97ef1-110">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="97ef1-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="97ef1-111">Prerequisites</span></span>

<span data-ttu-id="97ef1-112">Konfigurace integrace Azure AD s SAP HANA cloudové platformy identitu ověřování, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="97ef1-112">To configure Azure AD integration with SAP HANA Cloud Platform Identity Authentication, you need the following items:</span></span>

- <span data-ttu-id="97ef1-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="97ef1-113">An Azure AD subscription</span></span>
- <span data-ttu-id="97ef1-114">A **SAP HANA cloudové platformy identitu ověřování** předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="97ef1-114">A **SAP HANA Cloud Platform Identity Authentication** SSO enabled subscription</span></span>


>[!NOTE] 
><span data-ttu-id="97ef1-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="97ef1-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="97ef1-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="97ef1-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="97ef1-117">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="97ef1-117">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="97ef1-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat [zkušební verze jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="97ef1-118">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="97ef1-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="97ef1-119">Scenario description</span></span>
<span data-ttu-id="97ef1-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="97ef1-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="97ef1-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="97ef1-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="97ef1-122">Přidání ověření Identity SAP HANA cloudové platformy z Galerie</span><span class="sxs-lookup"><span data-stu-id="97ef1-122">Adding SAP HANA Cloud Platform Identity Authentication from the gallery</span></span>
2. <span data-ttu-id="97ef1-123">Konfigurace a testování Azure AD SSO</span><span class="sxs-lookup"><span data-stu-id="97ef1-123">Configuring and testing Azure AD SSO</span></span>

<span data-ttu-id="97ef1-124">Než začnete technické informace, je důležité pochopit, koncepty, které se chystáte podívejte se na.</span><span class="sxs-lookup"><span data-stu-id="97ef1-124">Before diving into the technical details, it is vital to understand the concepts you're going to look at.</span></span> <span data-ttu-id="97ef1-125">Ověření Identity SAP HANA cloudové platformy a Azure Active Directory federation umožňuje implementovat jednotného přihlašování napříč aplikacím nebo službám chráněným pomocí AAD (jako IdP) s aplikacemi SAP a službám chráněným pomocí SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-125">The SAP HANA Cloud Platform Identity Authentication and Azure Active Directory federation enables you to implement SSO across applications or services protected by AAD (as an IdP) with SAP applications and services protected by SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="97ef1-126">V současné době SAP HANA cloudové platformy identitu ověřování funguje jako zprostředkovatel Identity Proxy aplikací SAP.</span><span class="sxs-lookup"><span data-stu-id="97ef1-126">Currently, SAP HANA Cloud Platform Identity Authentication acts as a Proxy Identity Provider to SAP-applications.</span></span> <span data-ttu-id="97ef1-127">Azure Active Directory pak funguje jako počáteční zprostředkovatele Identity v rámci této instalace.</span><span class="sxs-lookup"><span data-stu-id="97ef1-127">Azure Active Directory in turn acts as the leading Identity Provider in this setup.</span></span> 

<span data-ttu-id="97ef1-128">Následující diagram znázorňuje toto:</span><span class="sxs-lookup"><span data-stu-id="97ef1-128">The following diagram illustrates this:</span></span>    

![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

<span data-ttu-id="97ef1-130">S tímto nastavením budou klientovi SAP HANA cloudové platformy identitu ověřování nakonfigurované jako důvěryhodné aplikace v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="97ef1-130">With this setup, your SAP HANA Cloud Platform Identity Authentication tenant will be configured as a trusted application in Azure Active Directory.</span></span> 

<span data-ttu-id="97ef1-131">Všechny aplikace SAP a službách, které chcete chránit pomocí tímto způsobem jsou následně nakonfigurované v konzole pro správu SAP HANA cloudové platformy identitu ověřování!</span><span class="sxs-lookup"><span data-stu-id="97ef1-131">All SAP applications and services you want to protect through this way are subsequently configured in the SAP HANA Cloud Platform Identity Authentication management console!</span></span>

<span data-ttu-id="97ef1-132">To znamená, že autorizace pro udělení přístupu k SAP aplikace a služby se musí provést v SAP HANA cloudové platformy identitu ověřování pro taková konfigurace (na rozdíl od konfigurace ověřování ve službě Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="97ef1-132">This means that authorization for granting access to SAP applications and services needs to take place in SAP HANA Cloud Platform Identity Authentication for such a setup (as opposed to configuring authorization in Azure Active Directory).</span></span>

<span data-ttu-id="97ef1-133">Konfigurace ověření Identity SAP HANA cloudové platformy jako aplikaci prostřednictvím Azure Active Directory Marketplace, nemusíte postará o konfiguraci potřebné jednotlivé deklaracích / SAML kontrolní výrazy a transformace potřebné k vytvoření platné ověřovací token pro aplikace SAP.</span><span class="sxs-lookup"><span data-stu-id="97ef1-133">By configuring SAP HANA Cloud Platform Identity Authentication as an application through the Azure Active Directory Marketplace, you don't need to take care of configuring needed individual claims / SAML assertions and transformations needed to produce a valid authentication token for SAP applications.</span></span>

>[!NOTE] 
><span data-ttu-id="97ef1-134">Obě strany, pouze byl testován aktuálně jednotného přihlašování k webu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-134">Currently Web SSO has been tested by both parties, only.</span></span> <span data-ttu-id="97ef1-135">Toky potřebné pro aplikaci API nebo rozhraní API API komunikace by měla fungovat, ale nebyly testovány, ještě.</span><span class="sxs-lookup"><span data-stu-id="97ef1-135">Flows needed for App-to-API or API-to-API communication should work but have not been tested, yet.</span></span> <span data-ttu-id="97ef1-136">Bude být testována jako součást následné aktivity.</span><span class="sxs-lookup"><span data-stu-id="97ef1-136">They will be tested as part of subsequent activities.</span></span>
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a><span data-ttu-id="97ef1-137">Přidat SAP HANA cloudové platformy identitu ověřování z Galerie</span><span class="sxs-lookup"><span data-stu-id="97ef1-137">Add SAP HANA Cloud Platform Identity Authentication from the gallery</span></span>
<span data-ttu-id="97ef1-138">Při konfiguraci integrace SAP HANA cloudové platformy identitu ověřování do služby Azure AD potřebujete přidat SAP HANA cloudové platformy identitu ověřování z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="97ef1-138">To configure the integration of SAP HANA Cloud Platform Identity Authentication into Azure AD, you need to add SAP HANA Cloud Platform Identity Authentication from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="97ef1-139">**Pokud chcete přidat SAP HANA cloudové platformy identitu ověřování z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97ef1-139">**To add SAP HANA Cloud Platform Identity Authentication from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="97ef1-140">V [ **portálu pro správu Azure**](https://portal.azure.com), v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-140">In the [**Azure Management portal**](https://portal.azure.com), on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="97ef1-142">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-142">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="97ef1-143">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-143">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="97ef1-145">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-145">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="97ef1-147">Do vyhledávacího pole zadejte **SAP HANA cloudové platformy identitu ověřování**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-147">In the search box, type **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. <span data-ttu-id="97ef1-149">Na panelu výsledků vyberte **SAP HANA cloudové platformy identitu ověřování**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97ef1-149">In the results panel, select **SAP HANA Cloud Platform Identity Authentication**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="97ef1-151">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="97ef1-151">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="97ef1-152">V této části konfiguraci a testování Azure AD přihlášení SSO se SAP HANA cloudové platformy Identity ověřování na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="97ef1-152">In this section, you configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="97ef1-153">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SAP HANA cloudové platformy Identity ověřování je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="97ef1-153">For SSO to work, Azure AD needs to know what the counterpart user in SAP HANA Cloud Platform Identity Authentication is to a user in Azure AD.</span></span> <span data-ttu-id="97ef1-154">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ověření Identity SAP HANA cloudové platformy je potřeba navázat.</span><span class="sxs-lookup"><span data-stu-id="97ef1-154">In other words, a link relationship between an Azure AD user and the related user in SAP HANA Cloud Platform Identity Authentication needs to be established.</span></span>

<span data-ttu-id="97ef1-155">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-155">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="97ef1-156">Nakonfigurovat a otestovat Azure AD přihlášení SSO se SAP HANA cloudové platformy identitu ověřování, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="97ef1-156">To configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="97ef1-157">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="97ef1-157">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="97ef1-158">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="97ef1-158">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="97ef1-159">**[Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  – Pokud chcete mít protějšek Britta Simon v SAP HANA cloudové platformy ověření Identity propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="97ef1-159">**[Creating a SAP HANA Cloud Platform Identity Authentication test user](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** - to have a counterpart of Britta Simon in SAP HANA Cloud Platform Identity Authentication that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="97ef1-160">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-160">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="97ef1-161">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="97ef1-161">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="97ef1-162">Konfigurace Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="97ef1-162">Configuring Azure AD SSO</span></span>

<span data-ttu-id="97ef1-163">V této části Povolení jednotného přihlašování Azure AD na portálu Azure Management portal a nakonfigurovat jednotné přihlašování v aplikaci SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-163">In this section, you enable Azure AD SSO in the Azure Management portal and configure single sign-on in your SAP HANA Cloud Platform Identity Authentication application.</span></span>

<span data-ttu-id="97ef1-164">Ověření Identity SAP HANA cloudové platformy aplikace očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-164">SAP HANA Cloud Platform Identity Authentication application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="97ef1-165">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="97ef1-165">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="97ef1-166">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="97ef1-166">The following screenshot shows an example for this.</span></span>

![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

<span data-ttu-id="97ef1-168">**Ke konfiguraci Azure AD jednotné přihlašování s SAP HANA cloudové platformy identitu ověřování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97ef1-168">**To configure Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, perform the following steps:**</span></span>

1. <span data-ttu-id="97ef1-169">Na portálu Azure Management portal na **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-169">In the Azure Management portal, on the **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="97ef1-171">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="97ef1-171">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování][5]

3. <span data-ttu-id="97ef1-173">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, pokud vaše aplikace SAP očekává atribut například "jméno".</span><span class="sxs-lookup"><span data-stu-id="97ef1-173">In the **User Attributes** section on the **Single sign-on** dialog, if your SAP application expects an attribute for example "firstName".</span></span> <span data-ttu-id="97ef1-174">V dialogovém okně atributy tokenu SAML přidejte atribut "jméno".</span><span class="sxs-lookup"><span data-stu-id="97ef1-174">On the SAML token attributes dialog, add the "firstName" attribute.</span></span>
 1. <span data-ttu-id="97ef1-175">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
 
    ![Konfigurovat jednotné přihlašování][6]

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. <span data-ttu-id="97ef1-178">V **název atributu** textovému poli, zadejte název atributu "jméno".</span><span class="sxs-lookup"><span data-stu-id="97ef1-178">In the **Attribute Name** textbox, type the attribute name "firstName".</span></span>
 3. <span data-ttu-id="97ef1-179">Z **hodnota atributu** vyberte hodnotu atributu "user.givenname".</span><span class="sxs-lookup"><span data-stu-id="97ef1-179">From the **Attribute Value** list, select the attribute value "user.givenname".</span></span>
 4. <span data-ttu-id="97ef1-180">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-180">Click **Ok**.</span></span>

4. <span data-ttu-id="97ef1-181">Na **SAP HANA cloudové platformy identitu ověřování domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97ef1-181">On the **SAP HANA Cloud Platform Identity Authentication Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. <span data-ttu-id="97ef1-183">V **přihlašovací adresa URL** textovému poli, zadejte přihlašovací na adresu URL pro aplikaci SAP.</span><span class="sxs-lookup"><span data-stu-id="97ef1-183">In the **Sign On URL** textbox, type the sign on URL for the SAP application.</span></span>
 2. <span data-ttu-id="97ef1-184">V **identifikátor** textovému poli, zadejte hodnotu následující vzoru:`<entity-id>.accounts.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="97ef1-184">In the **Identifier** textbox, type the value following pattern: `<entity-id>.accounts.ondemand.com`</span></span> 
    * <span data-ttu-id="97ef1-185">Pokud si nejste jisti tuto hodnotu, postupujte podle dokumentace SAP HANA cloudové platformy identitu ověřování v [konfiguraci klienta SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span><span class="sxs-lookup"><span data-stu-id="97ef1-185">If you don't know this value, please follow the SAP HANA Cloud Platform Identity Authentication documentation on [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span></span>

5. <span data-ttu-id="97ef1-186">Na **konfiguraci Identity ověřování cloudové platformy SAP HANA** klikněte na tlačítko **konfigurace SAP HANA cloudové platformy identitu ověřování** otevřete **konfigurovat přihlášení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-186">On the **SAP HANA Cloud Platform Identity Authentication Configuration** section, click **Configure SAP HANA Cloud Platform Identity Authentication** to open **Configure sign-on** dialog.</span></span> <span data-ttu-id="97ef1-187">Potom klikněte na **SAML XML Metadata** a uložte soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="97ef1-187">Then, click on **SAML XML Metadata** and save the file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. <span data-ttu-id="97ef1-190">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přejděte na SAP HANA cloudové platformy identitu ověřování konzole pro správu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-190">To get SSO configured for your application, go to SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span> <span data-ttu-id="97ef1-191">Adresa URL má vzoru následující:`https://<tenant-id>.accounts.ondemand.com/admin`</span><span class="sxs-lookup"><span data-stu-id="97ef1-191">The URL has the following pattern: `https://<tenant-id>.accounts.ondemand.com/admin`</span></span>
 * <span data-ttu-id="97ef1-192">Potom postupujte podle dokumentace na ověření Identity SAP HANA cloudové platformy na [konfigurace Microsoft Azure AD jako podnikové poskytovatele Identity v SAP HANA cloudové platformy identitu ověřování](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span><span class="sxs-lookup"><span data-stu-id="97ef1-192">Then, follow the documentation on SAP HANA Cloud Platform Identity Authentication to [Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span></span> 

7. <span data-ttu-id="97ef1-193">V portálu pro správu Azure, klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97ef1-193">In the Azure Management portal, click **Save** button.</span></span>
8. <span data-ttu-id="97ef1-194">Následující kroky pokračujte, pouze pokud chcete přidat a povolení jednotného přihlašování pro jinou aplikaci SAP.</span><span class="sxs-lookup"><span data-stu-id="97ef1-194">Continue the following steps only if you want to add and enable SSO for another SAP application.</span></span> <span data-ttu-id="97ef1-195">Opakujte kroky v části "Přidání SAP HANA cloudové platformy identitu ověřování z Galerie" přidat další instanci SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-195">Repeat steps under the section “Adding SAP HANA Cloud Platform Identity Authentication from the gallery” to add another instance of SAP HANA Cloud Platform Identity Authentication.</span></span>
9. <span data-ttu-id="97ef1-196">Na portálu Azure Management portal na **SAP HANA cloudové platformy identitu ověřování** stránky integrace aplikací, klikněte na tlačítko **propojené přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-196">In the Azure Management portal, on the **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Linked Sign-on**.</span></span>

    ![Konfigurovat propojené přihlášení](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. <span data-ttu-id="97ef1-198">Potom uložte konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="97ef1-198">Then, save the configuration.</span></span>

>[!NOTE] 
><span data-ttu-id="97ef1-199">Nová aplikace bude využívat Konfigurace jednotného přihlašování pro předchozí aplikaci SAP.</span><span class="sxs-lookup"><span data-stu-id="97ef1-199">The new application will leverage the SSO configuration for the previous SAP application.</span></span> <span data-ttu-id="97ef1-200">Zkontrolujte prosím, že používáte stejné podnikové poskytovatelů identit v SAP HANA cloudové platformy identitu ověřování konzole pro správu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-200">Please make sure you use the same Corporate Identity Providers in the SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span>
>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="97ef1-201">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="97ef1-201">Create an Azure AD test user</span></span>
<span data-ttu-id="97ef1-202">Cílem této části je vytvoření zkušebního uživatele v názvem Britta Simon nového portálu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-202">The objective of this section is to create a test user in the new portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="97ef1-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97ef1-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="97ef1-205">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-205">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="97ef1-207">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="97ef1-207">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="97ef1-209">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-209">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="97ef1-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="97ef1-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. <span data-ttu-id="97ef1-213">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-213">In the **Name** textbox, type **BrittaSimon**.</span></span>
  2. <span data-ttu-id="97ef1-214">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="97ef1-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>
  3. <span data-ttu-id="97ef1-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-215">Select **Show Password** and write down the value of the **Password**.</span></span>
  4. <span data-ttu-id="97ef1-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-216">Click **Create**.</span></span> 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a><span data-ttu-id="97ef1-217">Vytvoření zkušebního uživatele SAP HANA cloudové platformy identitu ověřování</span><span class="sxs-lookup"><span data-stu-id="97ef1-217">Create a SAP HANA Cloud Platform Identity Authentication test user</span></span>

<span data-ttu-id="97ef1-218">Nemusíte vytvořit uživatele na SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-218">You don't need to create an user on SAP HANA Cloud Platform Identity Authentication.</span></span> <span data-ttu-id="97ef1-219">Uživatelé, kteří jsou v úložišti uživatele Azure AD můžete použít funkci jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-219">Users who are in the Azure AD user store can use the SSO functionality.</span></span>

<span data-ttu-id="97ef1-220">Ověření Identity SAP HANA cloudové platformy podporuje možnost federaci identit.</span><span class="sxs-lookup"><span data-stu-id="97ef1-220">SAP HANA Cloud Platform Identity Authentication supports the Identity Federation option.</span></span> <span data-ttu-id="97ef1-221">Tato možnost umožňuje aplikace a zjistit, pokud ověřený zprostředkovatelem podnikové identitě uživatele existovat v úložišti z SAP HANA cloudové platformy Identity ověření uživatele.</span><span class="sxs-lookup"><span data-stu-id="97ef1-221">This option allows the application to check if the users authenticated by the corporate identity provider exist in the user store of SAP HANA Cloud Platform Identity Authentication.</span></span> 

<span data-ttu-id="97ef1-222">Ve výchozím nastavení je zakázána možnost federaci identit.</span><span class="sxs-lookup"><span data-stu-id="97ef1-222">In the default setting, the Identity Federation option is disabled.</span></span> <span data-ttu-id="97ef1-223">Pokud je povoleno federaci identit, mohou pouze uživatelé, kteří jsou importovány v SAP HANA cloudové platformy identitu ověřování přístup k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97ef1-223">If Identity Federation is enabled, only the users that are imported in SAP HANA Cloud Platform Identity Authentication are able to access the application.</span></span> 

<span data-ttu-id="97ef1-224">Další informace o tom, jak povolit nebo zakázat federaci identit s SAP HANA cloudové platformy identitu ověřování najdete v části Povolit federaci identit s SAP HANA cloudové platformy identitu ověřování v [konfigurace federaci identit s úložiště systému SAP HANA cloudové platformy Identity ověření uživatele.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span><span class="sxs-lookup"><span data-stu-id="97ef1-224">For more information about how to enable or disable Identity Federation with SAP HANA Cloud Platform Identity Authentication, see Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication in [Configure Identity Federation with the User Store of SAP HANA Cloud Platform Identity Authentication.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="97ef1-225">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="97ef1-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="97ef1-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-226">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to SAP HANA Cloud Platform Identity Authentication.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="97ef1-228">**Pokud chcete přiřadit Britta Simon SAP HANA cloudové platformy identitu ověřování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="97ef1-228">**To assign Britta Simon to SAP HANA Cloud Platform Identity Authentication, perform the following steps:**</span></span>

1. <span data-ttu-id="97ef1-229">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-229">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="97ef1-231">V seznamu aplikací vyberte **SAP HANA cloudové platformy identitu ověřování**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-231">In the applications list, select **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. <span data-ttu-id="97ef1-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="97ef1-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="97ef1-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="97ef1-235">Click **Add** button.</span></span> <span data-ttu-id="97ef1-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="97ef1-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="97ef1-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="97ef1-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="97ef1-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="97ef1-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="test-single-sign-on"></a><span data-ttu-id="97ef1-241">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="97ef1-241">Test single sign-on</span></span>

<span data-ttu-id="97ef1-242">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="97ef1-242">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="97ef1-243">Když kliknete na dlaždici SAP HANA cloudové platformy Identity ověřování na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci SAP HANA cloudové platformy identitu ověřování.</span><span class="sxs-lookup"><span data-stu-id="97ef1-243">When you click the SAP HANA Cloud Platform Identity Authentication tile in the Access Panel, you should get automatically signed-on to your SAP HANA Cloud Platform Identity Authentication application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="97ef1-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="97ef1-244">Additional resources</span></span>

* [<span data-ttu-id="97ef1-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="97ef1-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="97ef1-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="97ef1-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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