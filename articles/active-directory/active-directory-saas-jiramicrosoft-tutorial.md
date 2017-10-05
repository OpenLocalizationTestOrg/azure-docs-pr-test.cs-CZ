---
title: "Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML JIRA microsoftem | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML JIRA společností Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0dc847b9-eec4-4c31-845e-0144ddedc4a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: b5f7813c8244d2964b6894ae49cd64e0ee71b704
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a><span data-ttu-id="513cf-103">Kurz: Integrace Azure Active Directory pomocí jednotného přihlašování SAML JIRA společností Microsoft</span><span class="sxs-lookup"><span data-stu-id="513cf-103">Tutorial: Azure Active Directory integration with JIRA SAML SSO by Microsoft</span></span>

<span data-ttu-id="513cf-104">V tomto kurzu zjistěte, jak integrovat jednotné přihlašování SAML JIRA společností Microsoft s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="513cf-104">In this tutorial, you learn how to integrate JIRA SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="513cf-105">Integrace jednotné přihlašování SAML JIRA společností Microsoft s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="513cf-105">Integrating JIRA SAML SSO by Microsoft with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="513cf-106">Můžete řídit ve službě Azure AD, který má přístup k jednotné přihlašování SAML JIRA společností Microsoft</span><span class="sxs-lookup"><span data-stu-id="513cf-106">You can control in Azure AD who has access to JIRA SAML SSO by Microsoft</span></span>
- <span data-ttu-id="513cf-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k jednotné přihlašování SAML JIRA společností Microsoft (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="513cf-107">You can enable your users to automatically get signed-on to JIRA SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="513cf-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="513cf-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="513cf-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="513cf-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="513cf-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="513cf-110">Prerequisites</span></span>

<span data-ttu-id="513cf-111">Ke konfiguraci integrace služby Azure AD pomocí jednotného přihlašování SAML JIRA společností Microsoft, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="513cf-111">To configure Azure AD integration with JIRA SAML SSO by Microsoft, you need the following items:</span></span>

- <span data-ttu-id="513cf-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="513cf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="513cf-113">Aplikace serveru JIRA nainstalovaný na 64bitovou verzi Windows serveru (místní nebo v cloudu IaaS infrastrukturu)</span><span class="sxs-lookup"><span data-stu-id="513cf-113">JIRA server application installed on a Windows 64-bit server (on premise or on the cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="513cf-114">JIRA server je povolen protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="513cf-114">JIRA server is HTTPS enabled</span></span>
- <span data-ttu-id="513cf-115">Všimněte si, že podporované verze pro modul plug-in JIRA jsou uvedené v následující části.</span><span class="sxs-lookup"><span data-stu-id="513cf-115">Note the supported versions for JIRA Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="513cf-116">JIRA server je dostupný na Internetu, zvláště na Azure AD přihlašovací stránku pro ověřování a měli schopný přijímat tokenu z Azure AD</span><span class="sxs-lookup"><span data-stu-id="513cf-116">JIRA server is reachable on internet particularly to Azure AD Login page for authentication and should able to receive the token from Azure AD</span></span>
- <span data-ttu-id="513cf-117">Přihlašovací údaje správce se nastavují v JIRA</span><span class="sxs-lookup"><span data-stu-id="513cf-117">Admin credentials are set up in JIRA</span></span>
- <span data-ttu-id="513cf-118">V JIRA je zakázáno WebSudo</span><span class="sxs-lookup"><span data-stu-id="513cf-118">WebSudo is disabled in JIRA</span></span>
- <span data-ttu-id="513cf-119">Testovací uživatel vytvořené v aplikaci JIRA server</span><span class="sxs-lookup"><span data-stu-id="513cf-119">Test user created in the JIRA server application</span></span>

> [!NOTE]
> <span data-ttu-id="513cf-120">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí JIRA.</span><span class="sxs-lookup"><span data-stu-id="513cf-120">To test the steps in this tutorial, we do not recommend using a production environment of JIRA.</span></span> <span data-ttu-id="513cf-121">Nejdřív otestovat integrace v vývoj nebo pracovní prostředí aplikace a pak pomocí produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="513cf-121">Test the integration first in development or staging environment of the application and then use the production environment.</span></span>

<span data-ttu-id="513cf-122">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="513cf-122">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="513cf-123">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="513cf-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="513cf-124">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="513cf-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-jira"></a><span data-ttu-id="513cf-125">Podporované verze systému JIRA</span><span class="sxs-lookup"><span data-stu-id="513cf-125">Supported versions of JIRA</span></span> 

<span data-ttu-id="513cf-126">Od nyní jsou podporovány následující verze JIRA:</span><span class="sxs-lookup"><span data-stu-id="513cf-126">As of now, following versions of JIRA are supported:</span></span>

- <span data-ttu-id="513cf-127">Základní JIRA a Software: 6.0 na 7.2.0</span><span class="sxs-lookup"><span data-stu-id="513cf-127">JIRA Core and Software: 6.0 to 7.2.0</span></span>
- <span data-ttu-id="513cf-128">JIRA technickou podporu: 3.2 k 3.0</span><span class="sxs-lookup"><span data-stu-id="513cf-128">JIRA Service Desk: 3.0 to 3.2</span></span>

## <a name="scenario-description"></a><span data-ttu-id="513cf-129">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="513cf-129">Scenario description</span></span>
<span data-ttu-id="513cf-130">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="513cf-130">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="513cf-131">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="513cf-131">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="513cf-132">Přidání jednotné přihlašování SAML JIRA společností Microsoft z Galerie</span><span class="sxs-lookup"><span data-stu-id="513cf-132">Adding JIRA SAML SSO by Microsoft from the gallery</span></span>
2. <span data-ttu-id="513cf-133">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="513cf-133">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a><span data-ttu-id="513cf-134">Přidání jednotné přihlašování SAML JIRA společností Microsoft z Galerie</span><span class="sxs-lookup"><span data-stu-id="513cf-134">Adding JIRA SAML SSO by Microsoft from the gallery</span></span>
<span data-ttu-id="513cf-135">Při konfiguraci integrace přihlašování SAML JIRA společností Microsoft do služby Azure AD, potřebujete přidat jednotné přihlašování SAML JIRA společností Microsoft z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="513cf-135">To configure the integration of JIRA SAML SSO by Microsoft into Azure AD, you need to add JIRA SAML SSO by Microsoft from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="513cf-136">**Pokud chcete přidat jednotné přihlašování SAML JIRA společností Microsoft z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="513cf-136">**To add JIRA SAML SSO by Microsoft from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="513cf-137">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="513cf-137">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="513cf-139">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="513cf-139">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="513cf-140">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="513cf-140">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="513cf-142">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="513cf-142">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="513cf-144">Do vyhledávacího pole zadejte **jednotné přihlašování SAML JIRA Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="513cf-144">In the search box, type **JIRA SAML SSO by Microsoft**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_search.png)

5. <span data-ttu-id="513cf-146">Na panelu výsledků vyberte **jednotné přihlašování SAML JIRA Microsoft**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="513cf-146">In the results panel, select **JIRA SAML SSO by Microsoft**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="513cf-148">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="513cf-148">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="513cf-149">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společnosti Microsoft podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="513cf-149">In this section, you configure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="513cf-150">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v jednotné přihlašování SAML JIRA společností Microsoft je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="513cf-150">For single sign-on to work, Azure AD needs to know what the counterpart user in JIRA SAML SSO by Microsoft is to a user in Azure AD.</span></span> <span data-ttu-id="513cf-151">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské JIRA SAML SSO společností Microsoft musí navázat.</span><span class="sxs-lookup"><span data-stu-id="513cf-151">In other words, a link relationship between an Azure AD user and the related user in JIRA SAML SSO by Microsoft needs to be established.</span></span>

<span data-ttu-id="513cf-152">V JIRA jednotné přihlašování SAML společností Microsoft, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="513cf-152">In JIRA SAML SSO by Microsoft, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="513cf-153">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společností Microsoft, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="513cf-153">To configure and test Azure AD single sign-on with JIRA SAML SSO by Microsoft, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="513cf-154">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="513cf-154">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="513cf-155">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="513cf-155">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="513cf-156">**[Vytváření jednotné přihlašování SAML JIRA uživatelem testovací Microsoft](#creating-a-jira-saml-sso-by-microsoft-test-user)**  – Pokud chcete mít protějšek Britta Simon v jednotné přihlašování SAML JIRA společnosti Microsoft, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="513cf-156">**[Creating a JIRA SAML SSO by Microsoft test user](#creating-a-jira-saml-sso-by-microsoft-test-user)** - to have a counterpart of Britta Simon in JIRA SAML SSO by Microsoft that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="513cf-157">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="513cf-157">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="513cf-158">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="513cf-158">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="513cf-159">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="513cf-159">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="513cf-160">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaší jednotné přihlašování SAML JIRA aplikací společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="513cf-160">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your JIRA SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="513cf-161">**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování SAML JIRA společností Microsoft, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="513cf-161">**To configure Azure AD single sign-on with JIRA SAML SSO by Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="513cf-162">Na portálu Azure na **jednotné přihlašování SAML JIRA Microsoft** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="513cf-162">In the Azure portal, on the **JIRA SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="513cf-164">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="513cf-164">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_samlbase.png)

3. <span data-ttu-id="513cf-166">Na **jednotné přihlašování SAML JIRA Microsoft Domain a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="513cf-166">On the **JIRA SAML SSO by Microsoft Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_url.png)

    <span data-ttu-id="513cf-168">a.</span><span class="sxs-lookup"><span data-stu-id="513cf-168">a.</span></span> <span data-ttu-id="513cf-169">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="513cf-169">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="513cf-170">b.</span><span class="sxs-lookup"><span data-stu-id="513cf-170">b.</span></span> <span data-ttu-id="513cf-171">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="513cf-171">In the **Identifier** textbox, type a URL using the following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="513cf-172">c.</span><span class="sxs-lookup"><span data-stu-id="513cf-172">c.</span></span> <span data-ttu-id="513cf-173">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="513cf-173">In the **Reply URL** textbox, type a URL using the following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="513cf-174">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="513cf-174">These values are not real.</span></span> <span data-ttu-id="513cf-175">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="513cf-175">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="513cf-176">Port je volitelný, v případě, že je adresa URL s názvem.</span><span class="sxs-lookup"><span data-stu-id="513cf-176">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="513cf-177">Tyto hodnoty jsou přijímány během konfigurace modulu plug-in Jira, který je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="513cf-177">These values are received during the configuration of Jira plugin, which is explained later in the tutorial.</span></span>
 
4. <span data-ttu-id="513cf-178">Ke generování **Metadata** adresu url, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="513cf-178">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="513cf-179">a.</span><span class="sxs-lookup"><span data-stu-id="513cf-179">a.</span></span> <span data-ttu-id="513cf-180">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="513cf-180">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="513cf-182">b.</span><span class="sxs-lookup"><span data-stu-id="513cf-182">b.</span></span> <span data-ttu-id="513cf-183">Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="513cf-183">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="513cf-185">c.</span><span class="sxs-lookup"><span data-stu-id="513cf-185">c.</span></span> <span data-ttu-id="513cf-186">Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="513cf-186">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="513cf-188">d.</span><span class="sxs-lookup"><span data-stu-id="513cf-188">d.</span></span> <span data-ttu-id="513cf-189">Nyní přejděte na stránku vlastností **jednotné přihlašování SAML JIRA Microsoft** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="513cf-189">Now go to the property page of **JIRA SAML SSO by Microsoft** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/appid.png)

    <span data-ttu-id="513cf-191">e.</span><span class="sxs-lookup"><span data-stu-id="513cf-191">e.</span></span> <span data-ttu-id="513cf-192">Vygenerovat **adresu URL metadat** pomocí následujícího vzorce: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` a zkopírujte tuto hodnotu v poznámkovém bloku, jak se později používá pro konfiguraci modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="513cf-192">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for the configuration of the plugin.</span></span>

5. <span data-ttu-id="513cf-193">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="513cf-193">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="513cf-195">Obraťte se na [Microsoft](mailto:waadpartners@microsoft.com) s následující informace pro modul plug-in JIRA.</span><span class="sxs-lookup"><span data-stu-id="513cf-195">Contact [Microsoft](mailto:waadpartners@microsoft.com) with the following information for the JIRA plugin.</span></span>
    
    *   <span data-ttu-id="513cf-196">Jméno zákazníka:</span><span class="sxs-lookup"><span data-stu-id="513cf-196">Customer Name:</span></span>
    *   <span data-ttu-id="513cf-197">Název domény:</span><span class="sxs-lookup"><span data-stu-id="513cf-197">Primary domain name:</span></span>
    *   <span data-ttu-id="513cf-198">Azure AD Premium: Ano/Ne (modul plug-in bude k dispozici pro zákazníka Free, Basic a skladová položka Premium)</span><span class="sxs-lookup"><span data-stu-id="513cf-198">Azure AD Premium: Yes/No (Plugin will be available to all     the customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="513cf-199">Počet uživatelů, kteří budou používat Tato integrace:</span><span class="sxs-lookup"><span data-stu-id="513cf-199">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="513cf-200">JIRA verze:</span><span class="sxs-lookup"><span data-stu-id="513cf-200">JIRA Version:</span></span>
    *   <span data-ttu-id="513cf-201">Poznámka:</span><span class="sxs-lookup"><span data-stu-id="513cf-201">Comments:</span></span>

7. <span data-ttu-id="513cf-202">V okně prohlížeče jiný web Přihlaste se k instanci JIRA jako správce.</span><span class="sxs-lookup"><span data-stu-id="513cf-202">In a different web browser window, log in to your JIRA instance as an administrator.</span></span>

8. <span data-ttu-id="513cf-203">Pozastavte ukazatel myši na ikonu a klikněte na **doplňky**.</span><span class="sxs-lookup"><span data-stu-id="513cf-203">Hover on cog and click the **Add-ons**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="513cf-205">Karta části doplňky, klikněte na tlačítko **spravovat doplňky**.</span><span class="sxs-lookup"><span data-stu-id="513cf-205">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon7.png)

10. <span data-ttu-id="513cf-207">Ručně odešlete modulu plug-in od společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="513cf-207">Manually upload the plugin provided by Microsoft.</span></span> <span data-ttu-id="513cf-208">Po instalaci modulu plug-in se zobrazí v **uživatel nainstaloval** doplňky části **spravovat rozšíření** části.</span><span class="sxs-lookup"><span data-stu-id="513cf-208">Once the plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="513cf-209">Klikněte na tlačítko **konfigurace** konfigurace nového modulu plug-in.</span><span class="sxs-lookup"><span data-stu-id="513cf-209">Click **Configure** to configure the new plugin.</span></span>

12. <span data-ttu-id="513cf-210">Proveďte následující kroky na stránce konfigurace:</span><span class="sxs-lookup"><span data-stu-id="513cf-210">Perform following steps on configuration page:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="513cf-212">a.</span><span class="sxs-lookup"><span data-stu-id="513cf-212">a.</span></span> <span data-ttu-id="513cf-213">V **adresu URL metadat** vložit **adresu URL metadat** generované z Azure AD a klikněte na tlačítko **vyřešit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="513cf-213">In **Metadata URL** paste the **Metadata URL** generated from Azure AD and click the **Resolve** button.</span></span> <span data-ttu-id="513cf-214">Přečte adresu URL metadat IdP a naplní všechny informace o pole.</span><span class="sxs-lookup"><span data-stu-id="513cf-214">It reads the IdP metadata URL and populates all the fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="513cf-215">Výchozí umístění SAML ID uživatele je identifikátor jména.</span><span class="sxs-lookup"><span data-stu-id="513cf-215">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="513cf-216">Můžete to změnit na možnost atribut a zadejte odpovídající název.</span><span class="sxs-lookup"><span data-stu-id="513cf-216">You can change this to an attribute option and enter the appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="513cf-217">Zkontrolujte, zda je namapovaný na aplikaci tak, aby se nezobrazí žádná chyba při řešení metadata jen jeden certifikát.</span><span class="sxs-lookup"><span data-stu-id="513cf-217">Ensure that there is only one certificate mapped against the app so that there is no error in resolving the metadata.</span></span> <span data-ttu-id="513cf-218">Pokud máte víc certifikátů, při řešení metadata, získá správce k chybě.</span><span class="sxs-lookup"><span data-stu-id="513cf-218">If there are multiple certificates, upon resolving the metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="513cf-219">b.</span><span class="sxs-lookup"><span data-stu-id="513cf-219">b.</span></span> <span data-ttu-id="513cf-220">Kopírování **identifikátor, adresa URL odpovědi a adresa URL přihlašování** hodnoty a vložte je do **identifikátor, adresa URL odpovědi a adresa URL přihlašování** textových polí v uvedeném pořadí v **jednotné přihlašování SAML JIRA Microsoft Domain a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="513cf-220">Copy the **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **JIRA SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="513cf-221">c.</span><span class="sxs-lookup"><span data-stu-id="513cf-221">c.</span></span> <span data-ttu-id="513cf-222">V **přihlašovací jméno tlačítko** zadejte název vaší organizace chce, aby se uživatelům zobrazit na přihlašovací obrazovku tlačítka.</span><span class="sxs-lookup"><span data-stu-id="513cf-222">In **Login Button Name** type the name of button your organization wants the users to see on login screen.</span></span>

    <span data-ttu-id="513cf-223">d.</span><span class="sxs-lookup"><span data-stu-id="513cf-223">d.</span></span> <span data-ttu-id="513cf-224">V **SAML uživatele ID umístění** vyberte buď **ID uživatele je v elementu NameIdentifier příkaz subjektu** nebo **ID uživatele je v elementu atribut**.</span><span class="sxs-lookup"><span data-stu-id="513cf-224">In **SAML User ID Locations** select either **User ID is in the NameIdentifier element of the Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="513cf-225">Toto ID musí být JIRA id uživatele.</span><span class="sxs-lookup"><span data-stu-id="513cf-225">This ID has to be the JIRA user id.</span></span> <span data-ttu-id="513cf-226">Pokud neodpovídá id uživatele, systém nedovolí přihlášení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="513cf-226">If the user id is not matched, then system will not allow users to log in.</span></span> 
    
    <span data-ttu-id="513cf-227">e.</span><span class="sxs-lookup"><span data-stu-id="513cf-227">e.</span></span> <span data-ttu-id="513cf-228">Pokud vyberete **ID uživatele je v elementu atribut** možnost, pak v **název atributu** textového pole zadejte název atributu, kde je očekávána Id uživatele.</span><span class="sxs-lookup"><span data-stu-id="513cf-228">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type the name of the attribute where User Id is expected.</span></span> 

    <span data-ttu-id="513cf-229">f.</span><span class="sxs-lookup"><span data-stu-id="513cf-229">f.</span></span> <span data-ttu-id="513cf-230">Pokud používáte federované domény (například služby AD FS atd.) s Azure AD, potom klikněte na **povolit zjišťování domovské sféry** řádku a nakonfigurovat **název domény**.</span><span class="sxs-lookup"><span data-stu-id="513cf-230">If you are using the federated domain (like ADFS etc.) with Azure AD, then click on the **Enable Home Realm Discovery** option and configure the **Domain Name**.</span></span>
    
    <span data-ttu-id="513cf-231">g.</span><span class="sxs-lookup"><span data-stu-id="513cf-231">g.</span></span> <span data-ttu-id="513cf-232">V **název domény** zadejte název domény zde v případě přihlášení na základě služby AD FS.</span><span class="sxs-lookup"><span data-stu-id="513cf-232">In **Domain Name** type the domain name here in case of the ADFS-based login.</span></span>

    <span data-ttu-id="513cf-233">h.</span><span class="sxs-lookup"><span data-stu-id="513cf-233">h.</span></span> <span data-ttu-id="513cf-234">Zkontrolujte **povolit jednotné přihlašování se** Pokud se chcete odhlásit z Azure AD, když se uživatel neodhlásí z JIRA.</span><span class="sxs-lookup"><span data-stu-id="513cf-234">Check **Enable Single Sign out** if you wish to log out from Azure AD when a user logs out from JIRA.</span></span> 

    <span data-ttu-id="513cf-235">i.</span><span class="sxs-lookup"><span data-stu-id="513cf-235">i.</span></span> <span data-ttu-id="513cf-236">Klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení.</span><span class="sxs-lookup"><span data-stu-id="513cf-236">Click **Save** button to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="513cf-237">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="513cf-237">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="513cf-238">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="513cf-238">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="513cf-239">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="513cf-239">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="513cf-240">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="513cf-240">Creating an Azure AD test user</span></span>
<span data-ttu-id="513cf-241">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="513cf-241">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="513cf-243">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="513cf-243">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="513cf-244">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="513cf-244">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="513cf-246">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="513cf-246">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="513cf-248">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="513cf-248">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="513cf-250">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="513cf-250">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jiramicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="513cf-252">a.</span><span class="sxs-lookup"><span data-stu-id="513cf-252">a.</span></span> <span data-ttu-id="513cf-253">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="513cf-253">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="513cf-254">b.</span><span class="sxs-lookup"><span data-stu-id="513cf-254">b.</span></span> <span data-ttu-id="513cf-255">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="513cf-255">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="513cf-256">c.</span><span class="sxs-lookup"><span data-stu-id="513cf-256">c.</span></span> <span data-ttu-id="513cf-257">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="513cf-257">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="513cf-258">d.</span><span class="sxs-lookup"><span data-stu-id="513cf-258">d.</span></span> <span data-ttu-id="513cf-259">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="513cf-259">Click **Create**.</span></span>
 
### <a name="creating-a-jira-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="513cf-260">Vytváření jednotné přihlašování SAML JIRA Microsoft testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="513cf-260">Creating a JIRA SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="513cf-261">Pokud chcete povolit uživatelům Azure AD přihlášení do JIRA na místním serveru, se musí být zřízená do jednotné přihlašování SAML JIRA společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="513cf-261">To enable Azure AD users to log in to JIRA on-premise server, they must be provisioned into JIRA SAML SSO by Microsoft.</span></span> <span data-ttu-id="513cf-262">Pro jednotné přihlašování SAML JIRA společností Microsoft zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="513cf-262">For JIRA SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="513cf-263">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="513cf-263">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="513cf-264">Přihlaste se k serveru na místní JIRA jako správce.</span><span class="sxs-lookup"><span data-stu-id="513cf-264">Log in to your JIRA on-premise server as an administrator.</span></span>

2. <span data-ttu-id="513cf-265">Pozastavte ukazatel myši na ikonu a klikněte na **Správa uživatelů**.</span><span class="sxs-lookup"><span data-stu-id="513cf-265">Hover on cog and click the **User management**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="513cf-267">Budete přesměrováni na stránku přístup správce k zadání **heslo** a klikněte na tlačítko **potvrdit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="513cf-267">You are redirected to Administrator Access page to enter **Password** and click **Confirm** button.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user2.png) 

4. <span data-ttu-id="513cf-269">V části **Správa uživatelů** oddíl, klikněte na **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="513cf-269">Under **User management** tab section, click **create user**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user3.png) 

5. <span data-ttu-id="513cf-271">Na **"Vytvořit nový uživatel"** dialogové okno proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="513cf-271">On the **“Create new user”** dialog page, perform the following steps:</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-jiramicrosoft-tutorial/user4.png) 

    <span data-ttu-id="513cf-273">a.</span><span class="sxs-lookup"><span data-stu-id="513cf-273">a.</span></span> <span data-ttu-id="513cf-274">V **e-mailová adresa** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="513cf-274">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="513cf-275">b.</span><span class="sxs-lookup"><span data-stu-id="513cf-275">b.</span></span> <span data-ttu-id="513cf-276">V **úplný název** textovému poli, úplný název typu uživatele jako Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="513cf-276">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>

    <span data-ttu-id="513cf-277">c.</span><span class="sxs-lookup"><span data-stu-id="513cf-277">c.</span></span> <span data-ttu-id="513cf-278">V **uživatelské jméno** jako typ e-mailu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="513cf-278">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="513cf-279">d.</span><span class="sxs-lookup"><span data-stu-id="513cf-279">d.</span></span> <span data-ttu-id="513cf-280">V **heslo** textovému poli, zadejte heslo uživatele.</span><span class="sxs-lookup"><span data-stu-id="513cf-280">In the **Password** textbox, type the password of user.</span></span>

    <span data-ttu-id="513cf-281">e.</span><span class="sxs-lookup"><span data-stu-id="513cf-281">e.</span></span> <span data-ttu-id="513cf-282">Klikněte na tlačítko **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="513cf-282">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="513cf-283">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="513cf-283">Assigning the Azure AD test user</span></span>

<span data-ttu-id="513cf-284">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu jednotné přihlašování SAML JIRA společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="513cf-284">In this section, you enable Britta Simon to use Azure single sign-on by granting access to JIRA SAML SSO by Microsoft.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="513cf-286">**Pokud chcete přiřadit Britta Simon jednotné přihlašování SAML JIRA společností Microsoft, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="513cf-286">**To assign Britta Simon to JIRA SAML SSO by Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="513cf-287">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="513cf-287">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="513cf-289">V seznamu aplikací vyberte **jednotné přihlašování SAML JIRA Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="513cf-289">In the applications list, select **JIRA SAML SSO by Microsoft**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_jiramicrosoft_app.png) 

3. <span data-ttu-id="513cf-291">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="513cf-291">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="513cf-293">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="513cf-293">Click **Add** button.</span></span> <span data-ttu-id="513cf-294">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="513cf-294">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="513cf-296">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="513cf-296">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="513cf-297">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="513cf-297">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="513cf-298">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="513cf-298">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="513cf-299">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="513cf-299">Testing single sign-on</span></span>

<span data-ttu-id="513cf-300">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="513cf-300">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="513cf-301">Po kliknutí na tlačítko jednotné přihlašování SAML JIRA podle Microsoft dlaždice na přístupovém panelu jste měli získat automaticky přihlášení k vaší jednotné přihlašování SAML JIRA aplikací Microsoft.</span><span class="sxs-lookup"><span data-stu-id="513cf-301">When you click the JIRA SAML SSO by Microsoft tile in the Access Panel, you should get automatically signed-on to your JIRA SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="513cf-302">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="513cf-302">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="513cf-303">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="513cf-303">Additional resources</span></span>

* [<span data-ttu-id="513cf-304">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="513cf-304">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="513cf-305">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="513cf-305">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jiramicrosoft-tutorial/tutorial_general_203.png

