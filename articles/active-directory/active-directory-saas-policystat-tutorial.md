---
title: 'Kurz: Azure Active Directory integrace s PolicyStat | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a PolicyStat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 704afd5515b02ce2a4fbf35da65fad74dc506271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a><span data-ttu-id="32984-103">Kurz: Azure Active Directory integrace s PolicyStat</span><span class="sxs-lookup"><span data-stu-id="32984-103">Tutorial: Azure Active Directory integration with PolicyStat</span></span>

<span data-ttu-id="32984-104">V tomto kurzu zjistěte, jak integrovat PolicyStat s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="32984-104">In this tutorial, you learn how to integrate PolicyStat with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="32984-105">Integrace PolicyStat s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="32984-105">Integrating PolicyStat with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="32984-106">Můžete řídit ve službě Azure AD, který má přístup k PolicyStat</span><span class="sxs-lookup"><span data-stu-id="32984-106">You can control in Azure AD who has access to PolicyStat</span></span>
- <span data-ttu-id="32984-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k PolicyStat (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="32984-107">You can enable your users to automatically get signed-on to PolicyStat (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="32984-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="32984-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="32984-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="32984-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="32984-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="32984-110">Prerequisites</span></span>

<span data-ttu-id="32984-111">Konfigurace integrace Azure AD s PolicyStat, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="32984-111">To configure Azure AD integration with PolicyStat, you need the following items:</span></span>

- <span data-ttu-id="32984-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="32984-112">An Azure AD subscription</span></span>
- <span data-ttu-id="32984-113">PolicyStat jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="32984-113">A PolicyStat single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="32984-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="32984-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="32984-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="32984-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="32984-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="32984-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="32984-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="32984-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="32984-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="32984-118">Scenario description</span></span>
<span data-ttu-id="32984-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="32984-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="32984-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="32984-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="32984-121">Přidání PolicyStat z Galerie</span><span class="sxs-lookup"><span data-stu-id="32984-121">Adding PolicyStat from the gallery</span></span>
2. <span data-ttu-id="32984-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="32984-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-policystat-from-the-gallery"></a><span data-ttu-id="32984-123">Přidání PolicyStat z Galerie</span><span class="sxs-lookup"><span data-stu-id="32984-123">Adding PolicyStat from the gallery</span></span>
<span data-ttu-id="32984-124">Při konfiguraci integrace PolicyStat do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS PolicyStat z galerie.</span><span class="sxs-lookup"><span data-stu-id="32984-124">To configure the integration of PolicyStat into Azure AD, you need to add PolicyStat from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="32984-125">**Pokud chcete přidat PolicyStat z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32984-125">**To add PolicyStat from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="32984-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="32984-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="32984-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="32984-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="32984-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="32984-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="32984-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32984-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="32984-133">Do vyhledávacího pole zadejte **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="32984-133">In the search box, type **PolicyStat**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. <span data-ttu-id="32984-135">Na panelu výsledků vyberte **PolicyStat**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="32984-135">In the results panel, select **PolicyStat**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="32984-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="32984-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="32984-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s PolicyStat podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="32984-138">In this section, you configure and test Azure AD single sign-on with PolicyStat based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="32984-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v PolicyStat je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="32984-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PolicyStat is to a user in Azure AD.</span></span> <span data-ttu-id="32984-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v PolicyStat musí navázat.</span><span class="sxs-lookup"><span data-stu-id="32984-140">In other words, a link relationship between an Azure AD user and the related user in PolicyStat needs to be established.</span></span>

<span data-ttu-id="32984-141">V PolicyStat, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="32984-141">In PolicyStat, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="32984-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s PolicyStat, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="32984-142">To configure and test Azure AD single sign-on with PolicyStat, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="32984-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="32984-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="32984-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="32984-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="32984-145">**[Vytvoření zkušebního uživatele PolicyStat](#creating-a-policystat-test-user)**  – Pokud chcete mít protějšek Britta Simon v PolicyStat propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="32984-145">**[Creating a PolicyStat test user](#creating-a-policystat-test-user)** - to have a counterpart of Britta Simon in PolicyStat that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="32984-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="32984-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="32984-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="32984-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="32984-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="32984-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="32984-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="32984-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PolicyStat application.</span></span>

<span data-ttu-id="32984-150">**Ke konfiguraci Azure AD jednotné přihlašování s PolicyStat, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32984-150">**To configure Azure AD single sign-on with PolicyStat, perform the following steps:**</span></span>

1. <span data-ttu-id="32984-151">Na portálu Azure na **PolicyStat** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="32984-151">In the Azure portal, on the **PolicyStat** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="32984-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="32984-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. <span data-ttu-id="32984-155">Na **PolicyStat domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32984-155">On the **PolicyStat Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    <span data-ttu-id="32984-157">a.</span><span class="sxs-lookup"><span data-stu-id="32984-157">a.</span></span> <span data-ttu-id="32984-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.policystat.com`</span><span class="sxs-lookup"><span data-stu-id="32984-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.policystat.com`</span></span>

    <span data-ttu-id="32984-159">b.</span><span class="sxs-lookup"><span data-stu-id="32984-159">b.</span></span> <span data-ttu-id="32984-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.policystat.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="32984-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.policystat.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="32984-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="32984-161">These values are not real.</span></span> <span data-ttu-id="32984-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="32984-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="32984-163">Obraťte se na [tým podpory PolicyStat klienta](http://www.policystat.com/support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="32984-163">Contact [PolicyStat Client support team](http://www.policystat.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="32984-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="32984-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. <span data-ttu-id="32984-166">Cílem této části se popisují, jak uživatelům povolit ověřování na PolicyStat ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="32984-166">The objective of this section is to outline how to enable users to authenticate to PolicyStat with their account in Azure AD using federation based on the SAML protocol.</span></span>

    <span data-ttu-id="32984-167">Aplikace PolicyStat očekává SAML kontrolní výrazy ve specifickém formátu, který můžete přidat mapování vlastní atribut vyžaduje vaše **atributy tokenu SAML** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="32984-167">The PolicyStat application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span>  

     <span data-ttu-id="32984-168">Následující snímek obrazovky ukazuje příklad tohoto objektu.</span><span class="sxs-lookup"><span data-stu-id="32984-168">The following screenshot shows an example of this.</span></span>

     <span data-ttu-id="32984-169">![Atributy](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="32984-169">![Attributes](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "Attributes")</span></span>

6. <span data-ttu-id="32984-170">Pokud chcete přidat mapování požadovaný atribut, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32984-170">To add the required attribute mappings, perform the following steps:</span></span>

    | <span data-ttu-id="32984-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="32984-171">Attribute Name</span></span>    |   <span data-ttu-id="32984-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="32984-172">Attribute Value</span></span> |
    |------------------- | -------------------- |
    | <span data-ttu-id="32984-173">UID</span><span class="sxs-lookup"><span data-stu-id="32984-173">uid</span></span> | <span data-ttu-id="32984-174">ExtractMailPrefix([mail])</span><span class="sxs-lookup"><span data-stu-id="32984-174">ExtractMailPrefix([mail])</span></span> |
    
    <span data-ttu-id="32984-175">a.</span><span class="sxs-lookup"><span data-stu-id="32984-175">a.</span></span> <span data-ttu-id="32984-176">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32984-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    <span data-ttu-id="32984-179">b.</span><span class="sxs-lookup"><span data-stu-id="32984-179">b.</span></span> <span data-ttu-id="32984-180">V **název atributu** textovému poli, typ **uid**.</span><span class="sxs-lookup"><span data-stu-id="32984-180">In the **Attribute Name** textbox, type **uid**.</span></span>

    <span data-ttu-id="32984-181">c.</span><span class="sxs-lookup"><span data-stu-id="32984-181">c.</span></span> <span data-ttu-id="32984-182">V **hodnota atributu** textovému poli, vyberte **ExtractMailPrefix()**.</span><span class="sxs-lookup"><span data-stu-id="32984-182">In the **Attribute Value** textbox, select **ExtractMailPrefix()**.</span></span>    
   
    <span data-ttu-id="32984-183">d.</span><span class="sxs-lookup"><span data-stu-id="32984-183">d.</span></span> <span data-ttu-id="32984-184">Z **e-mailu** seznamu, vyberte **User.mail**.</span><span class="sxs-lookup"><span data-stu-id="32984-184">From the **Mail** list, select **User.mail**.</span></span>
    
    <span data-ttu-id="32984-185">e.</span><span class="sxs-lookup"><span data-stu-id="32984-185">e.</span></span> <span data-ttu-id="32984-186">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="32984-186">Click **Ok**</span></span>

7. <span data-ttu-id="32984-187">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="32984-187">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="32984-189">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="32984-189">In a different web browser window, log into your PolicyStat company site as an administrator.</span></span>

9. <span data-ttu-id="32984-190">Klikněte **správce** a pak klikněte **konfigurace přihlášení** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="32984-190">Click the **Admin** tab, and then click **Single Sign-On Configuration** in left navigation pane.</span></span>
   
    <span data-ttu-id="32984-191">![Správce nabídek](./media/active-directory-saas-policystat-tutorial/ic808633.png "správce nabídek")</span><span class="sxs-lookup"><span data-stu-id="32984-191">![Administrator Menu](./media/active-directory-saas-policystat-tutorial/ic808633.png "Administrator Menu")</span></span>

10. <span data-ttu-id="32984-192">V **instalace** vyberte **povolit jeden integrace přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="32984-192">In the **Setup** section, select **Enable Single Sign-on Integration**.</span></span>
   
    <span data-ttu-id="32984-193">![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808634.png "jednotné přihlašování v konfiguraci")</span><span class="sxs-lookup"><span data-stu-id="32984-193">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On Configuration")</span></span>

11. <span data-ttu-id="32984-194">Klikněte na tlačítko **konfigurace atributů**a pak na **konfigurace atributů** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32984-194">Click **Configure Attributes**, and then, in the **Configure Attributes** section, perform the following steps:</span></span>
   
    <span data-ttu-id="32984-195">![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808635.png "jednotné přihlašování v konfiguraci")</span><span class="sxs-lookup"><span data-stu-id="32984-195">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="32984-196">a.</span><span class="sxs-lookup"><span data-stu-id="32984-196">a.</span></span> <span data-ttu-id="32984-197">V **uživatelské jméno atribut** textovému poli, typ **uid**.</span><span class="sxs-lookup"><span data-stu-id="32984-197">In the **Username Attribute** textbox, type **uid**.</span></span>

    <span data-ttu-id="32984-198">b.</span><span class="sxs-lookup"><span data-stu-id="32984-198">b.</span></span> <span data-ttu-id="32984-199">V **křestní jméno atribut** textovému poli, typ **firstname** uživatele **Britta**.</span><span class="sxs-lookup"><span data-stu-id="32984-199">In the **First Name Attribute** textbox, type **firstname** of user **Britta**.</span></span>

    <span data-ttu-id="32984-200">c.</span><span class="sxs-lookup"><span data-stu-id="32984-200">c.</span></span> <span data-ttu-id="32984-201">V **poslední atribut Name** textovému poli, typ **lastname** uživatele **Simon**.</span><span class="sxs-lookup"><span data-stu-id="32984-201">In the **Last Name Attribute** textbox, type **lastname** of user **Simon**.</span></span>

    <span data-ttu-id="32984-202">d.</span><span class="sxs-lookup"><span data-stu-id="32984-202">d.</span></span> <span data-ttu-id="32984-203">V **atribut e-mailu** textovému poli, typ **emailaddress** uživatele  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="32984-203">In the **Email Attribute** textbox, type **emailaddress** of user **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="32984-204">e.</span><span class="sxs-lookup"><span data-stu-id="32984-204">e.</span></span> <span data-ttu-id="32984-205">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="32984-205">Click **Save Changes**.</span></span>

12. <span data-ttu-id="32984-206">Klikněte na tlačítko **si Metadata IDP**a pak na **si Metadata IDP** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32984-206">Click **Your IDP Metadata**, and then, in the **Your IDP Metadata** section, perform the following steps:</span></span>
   
    <span data-ttu-id="32984-207">![Jednotné přihlašování konfigurace](./media/active-directory-saas-policystat-tutorial/ic808636.png "jednotné přihlašování v konfiguraci")</span><span class="sxs-lookup"><span data-stu-id="32984-207">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="32984-208">a.</span><span class="sxs-lookup"><span data-stu-id="32984-208">a.</span></span> <span data-ttu-id="32984-209">Otevřete váš soubor stažený metadata, kopírovat obsah a vložte ji do **vaše metadat zprostředkovatelů Identity** textové pole.</span><span class="sxs-lookup"><span data-stu-id="32984-209">Open your downloaded metadata file, copy the content, and  then paste it into the **Your Identity Provider Metadata** textbox.</span></span>

    <span data-ttu-id="32984-210">b.</span><span class="sxs-lookup"><span data-stu-id="32984-210">b.</span></span> <span data-ttu-id="32984-211">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="32984-211">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="32984-212">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="32984-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="32984-213">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="32984-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="32984-214">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="32984-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="32984-215">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="32984-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="32984-216">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="32984-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="32984-218">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32984-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="32984-219">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="32984-219">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="32984-221">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="32984-221">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="32984-223">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32984-223">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="32984-225">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="32984-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="32984-227">a.</span><span class="sxs-lookup"><span data-stu-id="32984-227">a.</span></span> <span data-ttu-id="32984-228">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="32984-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="32984-229">b.</span><span class="sxs-lookup"><span data-stu-id="32984-229">b.</span></span> <span data-ttu-id="32984-230">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="32984-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="32984-231">c.</span><span class="sxs-lookup"><span data-stu-id="32984-231">c.</span></span> <span data-ttu-id="32984-232">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="32984-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="32984-233">d.</span><span class="sxs-lookup"><span data-stu-id="32984-233">d.</span></span> <span data-ttu-id="32984-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="32984-234">Click **Create**.</span></span>
 
### <a name="creating-a-policystat-test-user"></a><span data-ttu-id="32984-235">Vytvoření zkušebního uživatele PolicyStat</span><span class="sxs-lookup"><span data-stu-id="32984-235">Creating a PolicyStat test user</span></span>

<span data-ttu-id="32984-236">Pokud chcete povolit uživatelům Azure AD přihlášení do PolicyStat, musí být zřízená do PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="32984-236">In order to enable Azure AD users to log into PolicyStat, they must be provisioned into PolicyStat.</span></span>  

<span data-ttu-id="32984-237">PolicyStat podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="32984-237">PolicyStat supports just in time user provisioning.</span></span> <span data-ttu-id="32984-238">To znamená, není nutné ručně přidat uživatele do PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="32984-238">This means, you do not need to add the users manually to PolicyStat.</span></span> <span data-ttu-id="32984-239">Uživatelé bude se automaticky přidají na jejich první přihlášení pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="32984-239">The users will get added automatically on their first login through SSO.</span></span>

>[!NOTE]
><span data-ttu-id="32984-240">Můžete použít všechny ostatní PolicyStat uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované PolicyStat ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="32984-240">You can use any other PolicyStat user account creation tools or APIs provided by PolicyStat to provision Azure AD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="32984-241">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="32984-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="32984-242">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="32984-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PolicyStat.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="32984-244">**Pokud chcete přiřadit Britta Simon PolicyStat, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="32984-244">**To assign Britta Simon to PolicyStat, perform the following steps:**</span></span>

1. <span data-ttu-id="32984-245">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="32984-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="32984-247">V seznamu aplikací vyberte **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="32984-247">In the applications list, select **PolicyStat**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. <span data-ttu-id="32984-249">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="32984-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="32984-251">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="32984-251">Click **Add** button.</span></span> <span data-ttu-id="32984-252">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32984-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="32984-254">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="32984-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="32984-255">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32984-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="32984-256">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="32984-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="32984-257">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="32984-257">Testing single sign-on</span></span>

<span data-ttu-id="32984-258">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="32984-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="32984-259">Když kliknete na dlaždici PolicyStat na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="32984-259">When you click the PolicyStat tile in the Access Panel, you should get automatically signed-on to your PolicyStat application.</span></span>
<span data-ttu-id="32984-260">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="32984-260">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="32984-261">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="32984-261">Additional resources</span></span>

* [<span data-ttu-id="32984-262">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="32984-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="32984-263">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="32984-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

