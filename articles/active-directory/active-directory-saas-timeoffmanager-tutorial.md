---
title: 'Kurz: Azure Active Directory integrace s TimeOffManager | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TimeOffManager."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 3f944ffbf704694b293b4b1e5bdb4f2c93ae35a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a><span data-ttu-id="809a5-103">Kurz: Azure Active Directory integrace s TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="809a5-103">Tutorial: Azure Active Directory integration with TimeOffManager</span></span>

<span data-ttu-id="809a5-104">V tomto kurzu zjistěte, jak integrovat TimeOffManager s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="809a5-104">In this tutorial, you learn how to integrate TimeOffManager with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="809a5-105">Integrace TimeOffManager s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="809a5-105">Integrating TimeOffManager with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="809a5-106">Můžete řídit ve službě Azure AD, který má přístup k TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="809a5-106">You can control in Azure AD who has access to TimeOffManager</span></span>
- <span data-ttu-id="809a5-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TimeOffManager (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="809a5-107">You can enable your users to automatically get signed-on to TimeOffManager (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="809a5-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="809a5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="809a5-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="809a5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="809a5-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="809a5-110">Prerequisites</span></span>

<span data-ttu-id="809a5-111">Konfigurace integrace Azure AD s TimeOffManager, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="809a5-111">To configure Azure AD integration with TimeOffManager, you need the following items:</span></span>

- <span data-ttu-id="809a5-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="809a5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="809a5-113">TimeOffManager jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="809a5-113">A TimeOffManager single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="809a5-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="809a5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="809a5-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="809a5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="809a5-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="809a5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="809a5-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="809a5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="809a5-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="809a5-118">Scenario description</span></span>
<span data-ttu-id="809a5-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="809a5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="809a5-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="809a5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="809a5-121">Přidat TimeOffManager z Galerie</span><span class="sxs-lookup"><span data-stu-id="809a5-121">Add TimeOffManager from the gallery</span></span>
2. <span data-ttu-id="809a5-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="809a5-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-timeoffmanager-from-the-gallery"></a><span data-ttu-id="809a5-123">Přidat TimeOffManager z Galerie</span><span class="sxs-lookup"><span data-stu-id="809a5-123">Add TimeOffManager from the gallery</span></span>
<span data-ttu-id="809a5-124">Při konfiguraci integrace TimeOffManager do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS TimeOffManager z galerie.</span><span class="sxs-lookup"><span data-stu-id="809a5-124">To configure the integration of TimeOffManager into Azure AD, you need to add TimeOffManager from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="809a5-125">**Pokud chcete přidat TimeOffManager z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="809a5-125">**To add TimeOffManager from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="809a5-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="809a5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="809a5-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="809a5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="809a5-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="809a5-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="809a5-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="809a5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="809a5-133">Do vyhledávacího pole zadejte **TimeOffManager**, vyberte **TimeOffManager** z panelu výsledek a pak klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="809a5-133">In the search box, type **TimeOffManager**, select **TimeOffManager** from result panel and then click **Add** button to add the application.</span></span>

    ![Přidat z Galerie](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="809a5-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="809a5-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="809a5-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TimeOffManager podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="809a5-136">In this section, you configure and test Azure AD single sign-on with TimeOffManager based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="809a5-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v TimeOffManager je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="809a5-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TimeOffManager is to a user in Azure AD.</span></span> <span data-ttu-id="809a5-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TimeOffManager musí navázat.</span><span class="sxs-lookup"><span data-stu-id="809a5-138">In other words, a link relationship between an Azure AD user and the related user in TimeOffManager needs to be established.</span></span>

<span data-ttu-id="809a5-139">V TimeOffManager, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="809a5-139">In TimeOffManager, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="809a5-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TimeOffManager, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="809a5-140">To configure and test Azure AD single sign-on with TimeOffManager, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="809a5-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="809a5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="809a5-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="809a5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="809a5-143">**[Vytvoření zkušebního uživatele TimeOffManager](#create-a-timeoffmanager-test-user)**  – Pokud chcete mít protějšek Britta Simon v TimeOffManager propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="809a5-143">**[Create a TimeOffManager test user](#create-a-timeoffmanager-test-user)** - to have a counterpart of Britta Simon in TimeOffManager that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="809a5-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="809a5-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="809a5-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="809a5-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="809a5-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="809a5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="809a5-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="809a5-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TimeOffManager application.</span></span>

<span data-ttu-id="809a5-148">**Ke konfiguraci Azure AD jednotné přihlašování s TimeOffManager, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="809a5-148">**To configure Azure AD single sign-on with TimeOffManager, perform the following steps:**</span></span>

1. <span data-ttu-id="809a5-149">Na portálu Azure na **TimeOffManager** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="809a5-149">In the Azure portal, on the **TimeOffManager** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="809a5-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="809a5-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Přihlášení na základě SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. <span data-ttu-id="809a5-153">Na **TimeOffManager domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="809a5-153">On the **TimeOffManager Domain and URLs** section, perform the following:</span></span>

     ![Část TimeOffManager domény a adresy URL](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    <span data-ttu-id="809a5-155">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span><span class="sxs-lookup"><span data-stu-id="809a5-155">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="809a5-156">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="809a5-156">This value is not real.</span></span> <span data-ttu-id="809a5-157">Aktualizujte tuto hodnotu s skutečná adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="809a5-157">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="809a5-158">Můžete získat tuto hodnotu z **jednotného přihlašování na stránce nastavení** kterého se vysvětluje dále v kurzu nebo kontaktujte [tým podpory TimeOffManager](http://www.timeoffmanager.com/contact-us.aspx).</span><span class="sxs-lookup"><span data-stu-id="809a5-158">You can get this value from **Single Sign on settings page** which is explained later in the tutorial or Contact [TimeOffManager support team](http://www.timeoffmanager.com/contact-us.aspx).</span></span>
 
4. <span data-ttu-id="809a5-159">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="809a5-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. <span data-ttu-id="809a5-161">Cílem této části se popisují, jak uživatelům povolit ověřování na TimeOffManger ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="809a5-161">The objective of this section is to outline how to enable users to authenticate to TimeOffManger with their account in Azure AD using federation based on the SAML protocol.</span></span>
    
    <span data-ttu-id="809a5-162">Aplikace TimeOffManger očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="809a5-162">Your TimeOffManger application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="809a5-163">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="809a5-163">The following screenshot shows an example for this.</span></span>

    <span data-ttu-id="809a5-164">![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "atributy tokenu saml")</span><span class="sxs-lookup"><span data-stu-id="809a5-164">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml token attributes")</span></span>
    
    | <span data-ttu-id="809a5-165">Název atributu</span><span class="sxs-lookup"><span data-stu-id="809a5-165">Attribute Name</span></span> | <span data-ttu-id="809a5-166">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="809a5-166">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="809a5-167">FirstName</span><span class="sxs-lookup"><span data-stu-id="809a5-167">Firstname</span></span> |<span data-ttu-id="809a5-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="809a5-168">User.givenname</span></span> |
    | <span data-ttu-id="809a5-169">Příjmení</span><span class="sxs-lookup"><span data-stu-id="809a5-169">Lastname</span></span> |<span data-ttu-id="809a5-170">User.Surname</span><span class="sxs-lookup"><span data-stu-id="809a5-170">User.surname</span></span> |
    | <span data-ttu-id="809a5-171">E-mail</span><span class="sxs-lookup"><span data-stu-id="809a5-171">Email</span></span> |<span data-ttu-id="809a5-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="809a5-172">User.mail</span></span> |
    
    <span data-ttu-id="809a5-173">a.</span><span class="sxs-lookup"><span data-stu-id="809a5-173">a.</span></span>  <span data-ttu-id="809a5-174">Pro každý řádek dat v předchozí tabulce, klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="809a5-174">For each data row in the table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="809a5-175">![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "atributy tokenu saml")</span><span class="sxs-lookup"><span data-stu-id="809a5-175">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml token attributes")</span></span>
    
    <span data-ttu-id="809a5-176">![atributy tokenu SAML](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "atributy tokenu saml")</span><span class="sxs-lookup"><span data-stu-id="809a5-176">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml token attributes")</span></span>
    
    <span data-ttu-id="809a5-177">b.</span><span class="sxs-lookup"><span data-stu-id="809a5-177">b.</span></span>  <span data-ttu-id="809a5-178">V **název atributu** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="809a5-178">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="809a5-179">c.</span><span class="sxs-lookup"><span data-stu-id="809a5-179">c.</span></span>  <span data-ttu-id="809a5-180">V **hodnota atributu** textovému poli, vyberte hodnotu atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="809a5-180">In the **Attribute Value** textbox, select the attribute  value shown for that row.</span></span>
    
    <span data-ttu-id="809a5-181">d.</span><span class="sxs-lookup"><span data-stu-id="809a5-181">d.</span></span>  <span data-ttu-id="809a5-182">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="809a5-182">Click **Ok**.</span></span>
    
6. <span data-ttu-id="809a5-183">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="809a5-183">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="809a5-185">Na **TimeOffManager konfigurace** klikněte na tlačítko **konfigurace TimeOffManager** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="809a5-185">On the **TimeOffManager Configuration** section, click **Configure TimeOffManager** to open **Configure sign-on** window.</span></span> <span data-ttu-id="809a5-186">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="809a5-186">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TimeOffManager konfigurační oddíl](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. <span data-ttu-id="809a5-188">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="809a5-188">In a different web browser window, log into your TimeOffManager company site as an administrator.</span></span>

9. <span data-ttu-id="809a5-189">Přejděte na **účet \> účet možnosti \> jednotné přihlašování v nastavení**.</span><span class="sxs-lookup"><span data-stu-id="809a5-189">Go to **Account \> Account Options \> Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="809a5-190">![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="809a5-190">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "Single Sign-On Settings")</span></span>
7. <span data-ttu-id="809a5-191">V **nastavení jednotného přihlašování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="809a5-191">In the **Single Sign-On Settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="809a5-192">![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="809a5-192">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="809a5-193">a.</span><span class="sxs-lookup"><span data-stu-id="809a5-193">a.</span></span> <span data-ttu-id="809a5-194">V poznámkovém bloku otevřete váš kódování base-64 kódovaného certifikátu, zkopírujte obsah ho do schránky a vložte celý certifikát do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="809a5-194">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>
   
   <span data-ttu-id="809a5-195">b.</span><span class="sxs-lookup"><span data-stu-id="809a5-195">b.</span></span> <span data-ttu-id="809a5-196">V **Idp vystavitele** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="809a5-196">In **Idp Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="809a5-197">c.</span><span class="sxs-lookup"><span data-stu-id="809a5-197">c.</span></span> <span data-ttu-id="809a5-198">V **adresu URL koncového bodu IdP** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="809a5-198">In **IdP Endpoint URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="809a5-199">d.</span><span class="sxs-lookup"><span data-stu-id="809a5-199">d.</span></span> <span data-ttu-id="809a5-200">Jako **vynutit SAML**, vyberte **ne**.</span><span class="sxs-lookup"><span data-stu-id="809a5-200">As **Enforce SAML**, select **No**.</span></span>
   
   <span data-ttu-id="809a5-201">e.</span><span class="sxs-lookup"><span data-stu-id="809a5-201">e.</span></span> <span data-ttu-id="809a5-202">Jako **automaticky vytvářet uživatelé**, vyberte **Ano**.</span><span class="sxs-lookup"><span data-stu-id="809a5-202">As **Auto-Create Users**, select **Yes**.</span></span>
   
   <span data-ttu-id="809a5-203">f.</span><span class="sxs-lookup"><span data-stu-id="809a5-203">f.</span></span> <span data-ttu-id="809a5-204">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="809a5-204">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="809a5-205">g.</span><span class="sxs-lookup"><span data-stu-id="809a5-205">g.</span></span> <span data-ttu-id="809a5-206">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="809a5-206">click **Save Changes**.</span></span>

11. <span data-ttu-id="809a5-207">V **jednotného přihlašování nastavení** stránky, zkopírujte hodnotu **adresa URL služby příjemce Assertion** a vložte jej do **adresa URL odpovědi** textového pole pod **TimeOffManager domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="809a5-207">In **Single Sign on settings** page, copy the value of **Assertion Consumer Service URL** and paste it in the **Reply URL** text box under **TimeOffManager Domain and URLs** section in Azure portal.</span></span> 

      <span data-ttu-id="809a5-208">![Jednotné přihlašování v nastavení](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="809a5-208">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "Single Sign-On Settings")</span></span>

> [!TIP]
> <span data-ttu-id="809a5-209">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="809a5-209">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="809a5-210">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="809a5-210">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="809a5-211">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="809a5-211">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="809a5-212">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="809a5-212">Create an Azure AD test user</span></span>
<span data-ttu-id="809a5-213">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="809a5-213">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="809a5-215">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="809a5-215">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="809a5-216">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="809a5-216">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="809a5-218">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="809a5-218">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Uživatelé a skupiny--> všechny uživatele](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="809a5-220">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="809a5-220">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="809a5-222">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="809a5-222">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Dialogové okno stránka uživatel](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="809a5-224">a.</span><span class="sxs-lookup"><span data-stu-id="809a5-224">a.</span></span> <span data-ttu-id="809a5-225">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="809a5-225">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="809a5-226">b.</span><span class="sxs-lookup"><span data-stu-id="809a5-226">b.</span></span> <span data-ttu-id="809a5-227">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="809a5-227">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="809a5-228">c.</span><span class="sxs-lookup"><span data-stu-id="809a5-228">c.</span></span> <span data-ttu-id="809a5-229">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="809a5-229">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="809a5-230">d.</span><span class="sxs-lookup"><span data-stu-id="809a5-230">d.</span></span> <span data-ttu-id="809a5-231">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="809a5-231">Click **Create**.</span></span>
 
### <a name="create-a-timeoffmanager-test-user"></a><span data-ttu-id="809a5-232">Vytvoření zkušebního uživatele TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="809a5-232">Create a TimeOffManager test user</span></span>

<span data-ttu-id="809a5-233">Pokud chcete povolit uživatelům Azure AD přihlášení do TimeOffManager, musí být zřízená k TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="809a5-233">In order to enable Azure AD users to log into TimeOffManager, they must be provisioned to TimeOffManager.</span></span>  

<span data-ttu-id="809a5-234">TimeOffManager podporuje jenom při zřizování uživatelů čas.</span><span class="sxs-lookup"><span data-stu-id="809a5-234">TimeOffManager supports just in time user provisioning.</span></span> <span data-ttu-id="809a5-235">Neexistuje žádná položka akce za vás.</span><span class="sxs-lookup"><span data-stu-id="809a5-235">There is no action item for you.</span></span>  

<span data-ttu-id="809a5-236">Uživatelé se přidávají automaticky během první přihlášení pomocí jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="809a5-236">The users are added automatically during the first login using single sign on.</span></span>

>[!NOTE]
><span data-ttu-id="809a5-237">Můžete použít všechny ostatní TimeOffManager uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované TimeOffManager ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="809a5-237">You can use any other TimeOffManager user account creation tools or APIs provided by TimeOffManager to provision Azure AD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="809a5-238">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="809a5-238">Assign the Azure AD test user</span></span>

<span data-ttu-id="809a5-239">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="809a5-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TimeOffManager.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="809a5-241">**Pokud chcete přiřadit Britta Simon TimeOffManager, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="809a5-241">**To assign Britta Simon to TimeOffManager, perform the following steps:**</span></span>

1. <span data-ttu-id="809a5-242">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="809a5-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="809a5-244">V seznamu aplikací vyberte **TimeOffManager**.</span><span class="sxs-lookup"><span data-stu-id="809a5-244">In the applications list, select **TimeOffManager**.</span></span>

    ![TimeOffManager v seznamu aplikací](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. <span data-ttu-id="809a5-246">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="809a5-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="809a5-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="809a5-248">Click **Add** button.</span></span> <span data-ttu-id="809a5-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="809a5-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="809a5-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="809a5-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="809a5-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="809a5-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="809a5-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="809a5-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="809a5-254">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="809a5-254">Test single sign-on</span></span>

<span data-ttu-id="809a5-255">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="809a5-255">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="809a5-256">Když kliknete na dlaždici TimeOffManager na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="809a5-256">When you click the TimeOffManager tile in the Access Panel, you should get automatically signed-on to your TimeOffManager application.</span></span> <span data-ttu-id="809a5-257">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="809a5-257">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="809a5-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="809a5-258">Additional resources</span></span>

* [<span data-ttu-id="809a5-259">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="809a5-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="809a5-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="809a5-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

