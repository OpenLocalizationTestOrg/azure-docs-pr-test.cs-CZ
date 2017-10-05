---
title: 'Kurz: Azure Active Directory integrace s Springer odkaz | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Springer odkaz."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58cdf029-bdc0-43c4-a469-b921c2a669bd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: jeedes
ms.openlocfilehash: b9aec6f8f293cdd31456a7f50e3efe792804c7c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springer-link"></a><span data-ttu-id="1a49e-103">Kurz: Azure Active Directory integrace s Springer odkaz</span><span class="sxs-lookup"><span data-stu-id="1a49e-103">Tutorial: Azure Active Directory integration with Springer Link</span></span>

<span data-ttu-id="1a49e-104">V tomto kurzu zjistěte, jak integrovat Springer odkaz s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1a49e-104">In this tutorial, you learn how to integrate Springer Link with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a49e-105">Integrace Springer odkaz s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="1a49e-105">Integrating Springer Link with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1a49e-106">Můžete ovládat ve službě Azure AD, který má přístup k Springer odkaz.</span><span class="sxs-lookup"><span data-stu-id="1a49e-106">You can control in Azure AD who has access to Springer Link.</span></span>
- <span data-ttu-id="1a49e-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Springer odkaz (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a49e-107">You can enable your users to automatically get signed-on to Springer Link (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1a49e-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1a49e-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="1a49e-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a49e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a49e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1a49e-110">Prerequisites</span></span>

<span data-ttu-id="1a49e-111">Ke konfiguraci integrace služby Azure AD s odkazem Springer, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="1a49e-111">To configure Azure AD integration with Springer Link, you need the following items:</span></span>

- <span data-ttu-id="1a49e-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a49e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a49e-113">Odkaz Springer jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="1a49e-113">A Springer Link single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a49e-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a49e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a49e-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="1a49e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a49e-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="1a49e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1a49e-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a49e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a49e-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="1a49e-118">Scenario description</span></span>
<span data-ttu-id="1a49e-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="1a49e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a49e-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="1a49e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a49e-121">Přidání Springer propojení z Galerie</span><span class="sxs-lookup"><span data-stu-id="1a49e-121">Adding Springer Link from the gallery</span></span>
2. <span data-ttu-id="1a49e-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1a49e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springer-link-from-the-gallery"></a><span data-ttu-id="1a49e-123">Přidání Springer propojení z Galerie</span><span class="sxs-lookup"><span data-stu-id="1a49e-123">Adding Springer Link from the gallery</span></span>
<span data-ttu-id="1a49e-124">Při konfiguraci integrace Springer propojení do služby Azure AD, musíte přidat odkaz na Springer z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="1a49e-124">To configure the integration of Springer Link into Azure AD, you need to add Springer Link from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1a49e-125">**Přidat odkaz Springer z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1a49e-125">**To add Springer Link from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1a49e-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="1a49e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="1a49e-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1a49e-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="1a49e-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1a49e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="1a49e-133">Do vyhledávacího pole zadejte **Springer odkaz**, vyberte **Springer odkaz** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1a49e-133">In the search box, type **Springer Link**, select **Springer Link** from result panel then click **Add** button to add the application.</span></span>

    ![Odkaz springer v seznamu výsledků](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1a49e-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1a49e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1a49e-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Springer odkaz na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="1a49e-136">In this section, you configure and test Azure AD single sign-on with Springer Link based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1a49e-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v Springer odkaz je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a49e-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Springer Link is to a user in Azure AD.</span></span> <span data-ttu-id="1a49e-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Springer propojení je nutné stanovit.</span><span class="sxs-lookup"><span data-stu-id="1a49e-138">In other words, a link relationship between an Azure AD user and the related user in Springer Link needs to be established.</span></span>

<span data-ttu-id="1a49e-139">V propojení Springer přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="1a49e-139">In Springer Link, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1a49e-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Springer odkaz, musíte dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="1a49e-140">To configure and test Azure AD single sign-on with Springer Link, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1a49e-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="1a49e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1a49e-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a49e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a49e-143">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1a49e-143">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
4. <span data-ttu-id="1a49e-144">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1a49e-144">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1a49e-145">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="1a49e-145">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1a49e-146">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Springer odkaz.</span><span class="sxs-lookup"><span data-stu-id="1a49e-146">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Springer Link application.</span></span>

<span data-ttu-id="1a49e-147">**Ke konfiguraci Azure AD jednotné přihlašování s odkazem Springer, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1a49e-147">**To configure Azure AD single sign-on with Springer Link, perform the following steps:**</span></span>

1. <span data-ttu-id="1a49e-148">Na portálu Azure na **Springer odkaz** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-148">In the Azure portal, on the **Springer Link** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="1a49e-150">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="1a49e-150">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_samlbase.png)

3. <span data-ttu-id="1a49e-152">Na **Springer odkaz domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="1a49e-152">On the **Springer Link Domain and URLs** section,  If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Springer odkaz domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url1.png)

    <span data-ttu-id="1a49e-154">a.</span><span class="sxs-lookup"><span data-stu-id="1a49e-154">a.</span></span> <span data-ttu-id="1a49e-155">V **identifikátor** textovému poli, zadejte adresu URL:`https://fsso.springer.com`</span><span class="sxs-lookup"><span data-stu-id="1a49e-155">In the **Identifier** textbox, type the URL: `https://fsso.springer.com`</span></span>

    <span data-ttu-id="1a49e-156">b.</span><span class="sxs-lookup"><span data-stu-id="1a49e-156">b.</span></span> <span data-ttu-id="1a49e-157">V **adresa URL odpovědi** textovému poli, zadejte adresu URL:`https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="1a49e-157">In the **Reply URL** textbox, type the URL: `https://fsso-qa1.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span></span>    

4. <span data-ttu-id="1a49e-158">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-158">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="1a49e-159">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="1a49e-159">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Springer odkaz domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_url.png)

    <span data-ttu-id="1a49e-161">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL:`https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="1a49e-161">In the **Sign-on URL** textbox, type the URL : `https://fsso.springer.com/federation/Consumer/metaAlias/SpringerServiceProvider`</span></span>    

5. <span data-ttu-id="1a49e-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1a49e-162">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-springerlink-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1a49e-164">Ke generování **Metadata** adresu url, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1a49e-164">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="1a49e-165">a.</span><span class="sxs-lookup"><span data-stu-id="1a49e-165">a.</span></span> <span data-ttu-id="1a49e-166">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-166">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appregistrations.png)
   
    <span data-ttu-id="1a49e-168">b.</span><span class="sxs-lookup"><span data-stu-id="1a49e-168">b.</span></span> <span data-ttu-id="1a49e-169">Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1a49e-169">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpointicon.png)

    <span data-ttu-id="1a49e-171">c.</span><span class="sxs-lookup"><span data-stu-id="1a49e-171">c.</span></span> <span data-ttu-id="1a49e-172">Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="1a49e-172">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_endpoint.png)
     
    <span data-ttu-id="1a49e-174">d.</span><span class="sxs-lookup"><span data-stu-id="1a49e-174">d.</span></span> <span data-ttu-id="1a49e-175">Nyní přejděte na stránku vlastností **Springer odkaz** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="1a49e-175">Now go to the property page of **Springer Link** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_appid.png)

    <span data-ttu-id="1a49e-177">e.</span><span class="sxs-lookup"><span data-stu-id="1a49e-177">e.</span></span> <span data-ttu-id="1a49e-178">Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="1a49e-178">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="1a49e-179">Konfigurace jednotného přihlašování na **Springer odkaz** straně, budete muset odeslat vygenerovaného **adresu URL metadat** k [tým podpory Springer odkaz](mailto:identity@springernature.com).</span><span class="sxs-lookup"><span data-stu-id="1a49e-179">To configure single sign-on on **Springer Link** side, you need to send the generated **Metadata URL** to [Springer Link support team](mailto:identity@springernature.com).</span></span>

> [!TIP]
> <span data-ttu-id="1a49e-180">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="1a49e-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1a49e-181">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="1a49e-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1a49e-182">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1a49e-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1a49e-183">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a49e-183">Create an Azure AD test user</span></span>

<span data-ttu-id="1a49e-184">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a49e-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="1a49e-186">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1a49e-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1a49e-187">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1a49e-187">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-springerlink-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1a49e-189">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-189">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-springerlink-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1a49e-191">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1a49e-191">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-springerlink-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1a49e-193">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1a49e-193">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-springerlink-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1a49e-195">a.</span><span class="sxs-lookup"><span data-stu-id="1a49e-195">a.</span></span> <span data-ttu-id="1a49e-196">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-196">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a49e-197">b.</span><span class="sxs-lookup"><span data-stu-id="1a49e-197">b.</span></span> <span data-ttu-id="1a49e-198">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a49e-198">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="1a49e-199">c.</span><span class="sxs-lookup"><span data-stu-id="1a49e-199">c.</span></span> <span data-ttu-id="1a49e-200">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="1a49e-200">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="1a49e-201">d.</span><span class="sxs-lookup"><span data-stu-id="1a49e-201">d.</span></span> <span data-ttu-id="1a49e-202">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-202">Click **Create**.</span></span>
 
### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="1a49e-203">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a49e-203">Assign the Azure AD test user</span></span>

<span data-ttu-id="1a49e-204">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Springer odkaz.</span><span class="sxs-lookup"><span data-stu-id="1a49e-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Springer Link.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="1a49e-206">**Pokud chcete přiřadit Britta Simon Springer odkaz, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="1a49e-206">**To assign Britta Simon to Springer Link, perform the following steps:**</span></span>

1. <span data-ttu-id="1a49e-207">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="1a49e-209">V seznamu aplikací vyberte **Springer odkaz**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-209">In the applications list, select **Springer Link**.</span></span>

    ![Odkaz Springer odkaz v seznamu aplikací](./media/active-directory-saas-springerlink-tutorial/tutorial_springerlink_app.png)  

3. <span data-ttu-id="1a49e-211">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="1a49e-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="1a49e-213">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1a49e-213">Click **Add** button.</span></span> <span data-ttu-id="1a49e-214">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1a49e-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="1a49e-216">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1a49e-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1a49e-217">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1a49e-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a49e-218">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="1a49e-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1a49e-219">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="1a49e-219">Test single sign-on</span></span>

<span data-ttu-id="1a49e-220">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="1a49e-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1a49e-221">Když kliknete na dlaždici Springer odkaz na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Springer odkaz.</span><span class="sxs-lookup"><span data-stu-id="1a49e-221">When you click the Springer Link tile in the Access Panel, you should get automatically signed-on to your Springer Link application.</span></span>
<span data-ttu-id="1a49e-222">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1a49e-222">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a49e-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="1a49e-223">Additional resources</span></span>

* [<span data-ttu-id="1a49e-224">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a49e-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a49e-225">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1a49e-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-springerlink-tutorial/tutorial_general_203.png

