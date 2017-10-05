---
title: 'Kurz: Azure Active Directory integrace s SpringCM | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SpringCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: edfd06a06c730597fee4569ca1ce29092b45244a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a><span data-ttu-id="0b190-103">Kurz: Azure Active Directory integrace s SpringCM</span><span class="sxs-lookup"><span data-stu-id="0b190-103">Tutorial: Azure Active Directory integration with SpringCM</span></span>

<span data-ttu-id="0b190-104">V tomto kurzu zjistěte, jak integrovat SpringCM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0b190-104">In this tutorial, you learn how to integrate SpringCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0b190-105">Integrace SpringCM s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="0b190-105">Integrating SpringCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0b190-106">Můžete řídit ve službě Azure AD, který má přístup k SpringCM</span><span class="sxs-lookup"><span data-stu-id="0b190-106">You can control in Azure AD who has access to SpringCM</span></span>
- <span data-ttu-id="0b190-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k SpringCM (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="0b190-107">You can enable your users to automatically get signed-on to SpringCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0b190-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0b190-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0b190-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0b190-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b190-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0b190-110">Prerequisites</span></span>

<span data-ttu-id="0b190-111">Konfigurace integrace Azure AD s SpringCM, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="0b190-111">To configure Azure AD integration with SpringCM, you need the following items:</span></span>

- <span data-ttu-id="0b190-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="0b190-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0b190-113">SpringCM jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="0b190-113">A SpringCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0b190-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="0b190-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0b190-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="0b190-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0b190-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="0b190-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0b190-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0b190-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0b190-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="0b190-118">Scenario description</span></span>
<span data-ttu-id="0b190-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="0b190-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0b190-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="0b190-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0b190-121">Přidání SpringCM z Galerie</span><span class="sxs-lookup"><span data-stu-id="0b190-121">Adding SpringCM from the gallery</span></span>
2. <span data-ttu-id="0b190-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0b190-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springcm-from-the-gallery"></a><span data-ttu-id="0b190-123">Přidání SpringCM z Galerie</span><span class="sxs-lookup"><span data-stu-id="0b190-123">Adding SpringCM from the gallery</span></span>
<span data-ttu-id="0b190-124">Při konfiguraci integrace SpringCM do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS SpringCM z galerie.</span><span class="sxs-lookup"><span data-stu-id="0b190-124">To configure the integration of SpringCM into Azure AD, you need to add SpringCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0b190-125">**Pokud chcete přidat SpringCM z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0b190-125">**To add SpringCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0b190-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0b190-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0b190-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="0b190-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0b190-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0b190-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="0b190-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0b190-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="0b190-133">Do vyhledávacího pole zadejte **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="0b190-133">In the search box, type **SpringCM**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. <span data-ttu-id="0b190-135">Na panelu výsledků vyberte **SpringCM**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0b190-135">In the results panel, select **SpringCM**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0b190-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="0b190-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0b190-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s SpringCM podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="0b190-138">In this section, you configure and test Azure AD single sign-on with SpringCM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0b190-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v SpringCM je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0b190-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SpringCM is to a user in Azure AD.</span></span> <span data-ttu-id="0b190-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v SpringCM musí navázat.</span><span class="sxs-lookup"><span data-stu-id="0b190-140">In other words, a link relationship between an Azure AD user and the related user in SpringCM needs to be established.</span></span>

<span data-ttu-id="0b190-141">V SpringCM, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="0b190-141">In SpringCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0b190-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SpringCM, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="0b190-142">To configure and test Azure AD single sign-on with SpringCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0b190-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="0b190-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0b190-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0b190-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0b190-145">**[Vytvoření zkušebního uživatele SpringCM](#creating-a-springcm-test-user)**  – Pokud chcete mít protějšek Britta Simon v SpringCM propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b190-145">**[Creating a SpringCM test user](#creating-a-springcm-test-user)** - to have a counterpart of Britta Simon in SpringCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0b190-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0b190-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0b190-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0b190-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0b190-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0b190-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0b190-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci SpringCM.</span><span class="sxs-lookup"><span data-stu-id="0b190-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SpringCM application.</span></span>

<span data-ttu-id="0b190-150">**Ke konfiguraci Azure AD jednotné přihlašování s SpringCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0b190-150">**To configure Azure AD single sign-on with SpringCM, perform the following steps:**</span></span>

1. <span data-ttu-id="0b190-151">Na portálu Azure na **SpringCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="0b190-151">In the Azure portal, on the **SpringCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="0b190-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0b190-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. <span data-ttu-id="0b190-155">Na **SpringCM domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0b190-155">On the **SpringCM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    <span data-ttu-id="0b190-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="0b190-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0b190-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="0b190-158">This value is not real.</span></span> <span data-ttu-id="0b190-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0b190-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="0b190-160">Obraťte se na [tým podpory SpringCM klienta](https://knowledge.springcm.com/support) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0b190-160">Contact [SpringCM Client support team](https://knowledge.springcm.com/support) to get this value.</span></span> 
 
4. <span data-ttu-id="0b190-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="0b190-161">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. <span data-ttu-id="0b190-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0b190-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0b190-165">Na **SpringCM konfigurace** klikněte na tlačítko **konfigurace SpringCM** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="0b190-165">On the **SpringCM Configuration** section, click **Configure SpringCM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0b190-166">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="0b190-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. <span data-ttu-id="0b190-168">V okně prohlížeče jiný web, přihlaste se k vaší **SpringCM** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="0b190-168">In a different web browser window, sign on to your **SpringCM** company site as administrator.</span></span>

8. <span data-ttu-id="0b190-169">V nabídce v horní části, klikněte na tlačítko **přejít na**, klikněte na tlačítko **Předvolby**a pak na **předvolby účtu** klikněte na tlačítko **jednotné přihlašování SAML**.</span><span class="sxs-lookup"><span data-stu-id="0b190-169">In the menu on the top, click **GO TO**, click **Preferences**, and then, in the **Account Preferences** section, click **SAML SSO**.</span></span>
   
    <span data-ttu-id="0b190-170">![JEDNOTNÉ PŘIHLAŠOVÁNÍ SAML](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "JEDNOTNÉ PŘIHLAŠOVÁNÍ SAML")</span><span class="sxs-lookup"><span data-stu-id="0b190-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span></span>

9. <span data-ttu-id="0b190-171">V části Konfigurace zprostředkovatele Identity proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0b190-171">In the Identity Provider Configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="0b190-172">![Konfigurace zprostředkovatele identity](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "konfigurace zprostředkovatele Identity")</span><span class="sxs-lookup"><span data-stu-id="0b190-172">![Identity Provider Configuration](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "Identity Provider Configuration")</span></span>
    
    <span data-ttu-id="0b190-173">a.</span><span class="sxs-lookup"><span data-stu-id="0b190-173">a.</span></span> <span data-ttu-id="0b190-174">Nahrajte certifikát stažený Azure Active Directory, klikněte na tlačítko **vybrat certifikát vystavitele** nebo **změnu vystavitele certifikátu**.</span><span class="sxs-lookup"><span data-stu-id="0b190-174">To upload your downloaded Azure Active Directory certificate, click **Select Issuer Certificate** or **Change Issuer Certificate**.</span></span>
    
    <span data-ttu-id="0b190-175">b.</span><span class="sxs-lookup"><span data-stu-id="0b190-175">b.</span></span> <span data-ttu-id="0b190-176">Vložení **SAML Entity ID** hodnotu, kterou jste zkopírovali z portálu Azure do **vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0b190-176">Paste **SAML Entity ID** value, which you have copied from Azure portal into the **Issuer** textbox.</span></span>
    
    <span data-ttu-id="0b190-177">c.</span><span class="sxs-lookup"><span data-stu-id="0b190-177">c.</span></span> <span data-ttu-id="0b190-178">Vložení **SAML jeden přihlašování adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure do **koncový bod iniciované poskytovatele služeb (SP)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="0b190-178">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **Service Provider (SP) Initiated Endpoint** textbox.</span></span>
            
    <span data-ttu-id="0b190-179">d.</span><span class="sxs-lookup"><span data-stu-id="0b190-179">d.</span></span> <span data-ttu-id="0b190-180">Vyberte **povoleno SAML** jako **povolit**.</span><span class="sxs-lookup"><span data-stu-id="0b190-180">Select **SAML Enabled** as **Enable**.</span></span>

    <span data-ttu-id="0b190-181">e.</span><span class="sxs-lookup"><span data-stu-id="0b190-181">e.</span></span> <span data-ttu-id="0b190-182">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0b190-182">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="0b190-183">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="0b190-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0b190-184">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="0b190-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0b190-185">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0b190-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0b190-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0b190-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="0b190-187">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0b190-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="0b190-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0b190-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0b190-190">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="0b190-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0b190-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="0b190-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0b190-194">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0b190-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0b190-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0b190-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0b190-198">a.</span><span class="sxs-lookup"><span data-stu-id="0b190-198">a.</span></span> <span data-ttu-id="0b190-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0b190-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0b190-200">b.</span><span class="sxs-lookup"><span data-stu-id="0b190-200">b.</span></span> <span data-ttu-id="0b190-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0b190-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0b190-202">c.</span><span class="sxs-lookup"><span data-stu-id="0b190-202">c.</span></span> <span data-ttu-id="0b190-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0b190-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0b190-204">d.</span><span class="sxs-lookup"><span data-stu-id="0b190-204">d.</span></span> <span data-ttu-id="0b190-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="0b190-205">Click **Create**.</span></span>
 
### <a name="creating-a-springcm-test-user"></a><span data-ttu-id="0b190-206">Vytvoření zkušebního uživatele SpringCM</span><span class="sxs-lookup"><span data-stu-id="0b190-206">Creating a SpringCM test user</span></span>

<span data-ttu-id="0b190-207">Pokud chcete povolit uživatelů Azure Active Directory k přihlášení do SpringCM, musí být zřízená do SpringCM.</span><span class="sxs-lookup"><span data-stu-id="0b190-207">To enable Azure Active Directory users to log in to SpringCM, they must be provisioned into SpringCM.</span></span> <span data-ttu-id="0b190-208">V případě SpringCM zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="0b190-208">In the case of SpringCM, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="0b190-209">Další informace najdete v tématu [vytvoření a úprava uživatele SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span><span class="sxs-lookup"><span data-stu-id="0b190-209">For more information, see [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span></span> 

<span data-ttu-id="0b190-210">**K poskytnutí uživatelského účtu do SpringCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0b190-210">**To provision a user account to SpringCM, perform the following steps:**</span></span>

1. <span data-ttu-id="0b190-211">Přihlaste se k vaší **SpringCM** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="0b190-211">Log in to your **SpringCM** company site as administrator.</span></span>

2. <span data-ttu-id="0b190-212">Klikněte na tlačítko **GOTO**a potom klikněte na **adresáře**.</span><span class="sxs-lookup"><span data-stu-id="0b190-212">Click **GOTO**, and then click **ADDRESS BOOK**.</span></span>
   
    <span data-ttu-id="0b190-213">![Vytvoření uživatele](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="0b190-213">![Create User](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "Create User")</span></span>

3. <span data-ttu-id="0b190-214">Klikněte na tlačítko **vytvořit uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0b190-214">Click **Create User**.</span></span>

4. <span data-ttu-id="0b190-215">Vyberte **Role uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0b190-215">Select a **User Role**.</span></span>

5. <span data-ttu-id="0b190-216">Vyberte **odeslání e-mailu aktivace**.</span><span class="sxs-lookup"><span data-stu-id="0b190-216">Select **Send Activation Email**.</span></span>

6. <span data-ttu-id="0b190-217">Zadejte jméno, příjmení a e-mailovou adresu platného účtu uživatele Azure Active Directory, který má být zahrnuty do související textových polí.</span><span class="sxs-lookup"><span data-stu-id="0b190-217">Type the first name, last name, and email address of a valid Azure Active Directory user account you want to provision into the related textboxes.</span></span>

7. <span data-ttu-id="0b190-218">Přidejte uživatele do **skupiny zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="0b190-218">Add the user to a **Security group**.</span></span>

8. <span data-ttu-id="0b190-219">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="0b190-219">Click **Save**.</span></span>

  >[!NOTE]
  ><span data-ttu-id="0b190-220">Můžete použít všechny ostatní SpringCM uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované SpringCM zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="0b190-220">You can use any other SpringCM user account creation tools or APIs provided by SpringCM to provision AAD user accounts.</span></span>  
  > 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0b190-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="0b190-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0b190-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu SpringCM.</span><span class="sxs-lookup"><span data-stu-id="0b190-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SpringCM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="0b190-224">**Pokud chcete přiřadit Britta Simon SpringCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="0b190-224">**To assign Britta Simon to SpringCM, perform the following steps:**</span></span>

1. <span data-ttu-id="0b190-225">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0b190-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="0b190-227">V seznamu aplikací vyberte **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="0b190-227">In the applications list, select **SpringCM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. <span data-ttu-id="0b190-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="0b190-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="0b190-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0b190-231">Click **Add** button.</span></span> <span data-ttu-id="0b190-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0b190-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="0b190-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0b190-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0b190-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0b190-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0b190-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0b190-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0b190-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="0b190-237">Testing single sign-on</span></span>

<span data-ttu-id="0b190-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="0b190-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="0b190-239">Když kliknete na dlaždici SpringCM na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci SpringCM.</span><span class="sxs-lookup"><span data-stu-id="0b190-239">When you click the SpringCM tile in the Access Panel, you should get automatically signed-on to your SpringCM application.</span></span>

<span data-ttu-id="0b190-240">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0b190-240">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0b190-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="0b190-241">Additional resources</span></span>

* [<span data-ttu-id="0b190-242">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0b190-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0b190-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0b190-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

