---
title: 'Kurz: Azure Active Directory integrace s TINFOIL SECURITY | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TINFOIL SECURITY."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 614e4de3335574f4b56c7d641af4fcfafdb17d12
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a><span data-ttu-id="db20a-103">Kurz: Azure Active Directory integrace s TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="db20a-103">Tutorial: Azure Active Directory integration with TINFOIL SECURITY</span></span>

<span data-ttu-id="db20a-104">V tomto kurzu zjistěte, jak integrovat TINFOIL SECURITY s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="db20a-104">In this tutorial, you learn how to integrate TINFOIL SECURITY with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="db20a-105">Integrace TINFOIL SECURITY s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="db20a-105">Integrating TINFOIL SECURITY with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="db20a-106">Můžete řídit ve službě Azure AD, který má přístup k TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="db20a-106">You can control in Azure AD who has access to TINFOIL SECURITY</span></span>
- <span data-ttu-id="db20a-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k TINFOIL SECURITY (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="db20a-107">You can enable your users to automatically get signed-on to TINFOIL SECURITY (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="db20a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="db20a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="db20a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="db20a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db20a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="db20a-110">Prerequisites</span></span>

<span data-ttu-id="db20a-111">Konfigurace integrace Azure AD s TINFOIL SECURITY, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="db20a-111">To configure Azure AD integration with TINFOIL SECURITY, you need the following items:</span></span>

- <span data-ttu-id="db20a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="db20a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="db20a-113">TINFOIL SECURITY jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="db20a-113">A TINFOIL SECURITY single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="db20a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="db20a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="db20a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="db20a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="db20a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="db20a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="db20a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="db20a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="db20a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="db20a-118">Scenario description</span></span>
<span data-ttu-id="db20a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="db20a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="db20a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="db20a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="db20a-121">Přidat TINFOIL SECURITY z Galerie</span><span class="sxs-lookup"><span data-stu-id="db20a-121">Add TINFOIL SECURITY from the gallery</span></span>
2. <span data-ttu-id="db20a-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db20a-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tinfoil-security-from-the-gallery"></a><span data-ttu-id="db20a-123">Přidat TINFOIL SECURITY z Galerie</span><span class="sxs-lookup"><span data-stu-id="db20a-123">Add TINFOIL SECURITY from the gallery</span></span>
<span data-ttu-id="db20a-124">Při konfiguraci integrace TINFOIL SECURITY do služby Azure AD potřebujete přidat TINFOIL SECURITY z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="db20a-124">To configure the integration of TINFOIL SECURITY into Azure AD, you need to add TINFOIL SECURITY from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="db20a-125">**Pokud chcete přidat TINFOIL SECURITY z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db20a-125">**To add TINFOIL SECURITY from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="db20a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="db20a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="db20a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="db20a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="db20a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="db20a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="db20a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db20a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="db20a-133">Do vyhledávacího pole zadejte **TINFOIL SECURITY**, vyberte **TINFOIL SECURITY** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db20a-133">In the search box, type **TINFOIL SECURITY**, select  **TINFOIL SECURITY** from result panel then click **Add** button to add the application.</span></span>

    ![TINFOIL SECURITY z Galerie](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="db20a-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db20a-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="db20a-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s TINFOIL SECURITY podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="db20a-136">In this section, you configure and test Azure AD single sign-on with TINFOIL SECURITY based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="db20a-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v TINFOIL SECURITY je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db20a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TINFOIL SECURITY is to a user in Azure AD.</span></span> <span data-ttu-id="db20a-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v TINFOIL SECURITY musí navázat.</span><span class="sxs-lookup"><span data-stu-id="db20a-138">In other words, a link relationship between an Azure AD user and the related user in TINFOIL SECURITY needs to be established.</span></span>

<span data-ttu-id="db20a-139">V TINFOIL SECURITY přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="db20a-139">In TINFOIL SECURITY, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="db20a-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TINFOIL SECURITY, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="db20a-140">To configure and test Azure AD single sign-on with TINFOIL SECURITY, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="db20a-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="db20a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="db20a-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db20a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="db20a-143">**[Vytvoření zkušebního uživatele TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  – Pokud chcete mít protějšek Britta Simon v TINFOIL SECURITY propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="db20a-143">**[Create a TINFOIL SECURITY test user](#create-a-tinfoil-security-test-user)** - to have a counterpart of Britta Simon in TINFOIL SECURITY that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="db20a-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db20a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="db20a-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="db20a-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="db20a-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="db20a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="db20a-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="db20a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TINFOIL SECURITY application.</span></span>

<span data-ttu-id="db20a-148">**Ke konfiguraci Azure AD jednotné přihlašování s TINFOIL SECURITY, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db20a-148">**To configure Azure AD single sign-on with TINFOIL SECURITY, perform the following steps:**</span></span>

1. <span data-ttu-id="db20a-149">Na portálu Azure na **TINFOIL SECURITY** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="db20a-149">In the Azure portal, on the **TINFOIL SECURITY** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="db20a-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="db20a-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Přihlášení na základě SAML](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. <span data-ttu-id="db20a-153">Na **TINFOIL zabezpečení domény a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="db20a-153">On the **TINFOIL SECURITY Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. <span data-ttu-id="db20a-155">Na **SAML podpisový certifikát** část, zkopírujte **kryptografický OTISK** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="db20a-155">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value.</span></span>

    ![Certifikát pro podpis SAML části](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. <span data-ttu-id="db20a-157">Pokud chcete přidat mapování požadovaný atribut, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db20a-157">To add the required attribute mappings, perform the following steps:</span></span>
    
    <span data-ttu-id="db20a-158">![Atributy](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="db20a-158">![Attributes](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "Attributes")</span></span>
    
    | <span data-ttu-id="db20a-159">Název atributu</span><span class="sxs-lookup"><span data-stu-id="db20a-159">Attribute Name</span></span>    |   <span data-ttu-id="db20a-160">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="db20a-160">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="db20a-161">ID účtu</span><span class="sxs-lookup"><span data-stu-id="db20a-161">accountid</span></span> | <span data-ttu-id="db20a-162">UXXXXXXXXXXXXX</span><span class="sxs-lookup"><span data-stu-id="db20a-162">UXXXXXXXXXXXXX</span></span> |
    
    <span data-ttu-id="db20a-163">a.</span><span class="sxs-lookup"><span data-stu-id="db20a-163">a.</span></span> <span data-ttu-id="db20a-164">Klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="db20a-164">Click **add user attribute**.</span></span>
    
    <span data-ttu-id="db20a-165">![Přidat atribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="db20a-165">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "Attributes")</span></span>
    
    <span data-ttu-id="db20a-166">![Přidat atribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="db20a-166">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "Attributes")</span></span>
    
    <span data-ttu-id="db20a-167">b.</span><span class="sxs-lookup"><span data-stu-id="db20a-167">b.</span></span> <span data-ttu-id="db20a-168">V **název atributu** textovému poli, typ **accountid**.</span><span class="sxs-lookup"><span data-stu-id="db20a-168">In the **Attribute Name** textbox, type **accountid**.</span></span>
    
    <span data-ttu-id="db20a-169">c.</span><span class="sxs-lookup"><span data-stu-id="db20a-169">c.</span></span> <span data-ttu-id="db20a-170">V **hodnota atributu** textovému poli, vložte ID účtu hodnotu, která budete mít později v kurzu.</span><span class="sxs-lookup"><span data-stu-id="db20a-170">In the **Attribute Value** textbox, paste the account ID value which you will get later on the tutorial.</span></span>
    
    <span data-ttu-id="db20a-171">d.</span><span class="sxs-lookup"><span data-stu-id="db20a-171">d.</span></span> <span data-ttu-id="db20a-172">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="db20a-172">Click **Ok**.</span></span>    

6. <span data-ttu-id="db20a-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db20a-173">Click **Save** button.</span></span>

    ![Tlačítko Uložit](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="db20a-175">Na **TINFOIL SECURITY Configuration** klikněte na tlačítko **konfigurace TINFOIL SECURITY** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="db20a-175">On the **TINFOIL SECURITY Configuration** section, click **Configure TINFOIL SECURITY** to open **Configure sign-on** window.</span></span> <span data-ttu-id="db20a-176">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="db20a-176">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace zabezpečení aplikace TINFOIL](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. <span data-ttu-id="db20a-178">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="db20a-178">In a different web browser window, log into your TINFOIL SECURITY company site as an administrator.</span></span>

9. <span data-ttu-id="db20a-179">Na panelu nástrojů v horní části klikněte na tlačítko **Můj účet**.</span><span class="sxs-lookup"><span data-stu-id="db20a-179">In the toolbar on the top, click **My Account**.</span></span>
   
    <span data-ttu-id="db20a-180">![Řídicí panel](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "řídicí panel")</span><span class="sxs-lookup"><span data-stu-id="db20a-180">![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Dashboard")</span></span>

10. <span data-ttu-id="db20a-181">Klikněte na tlačítko **zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="db20a-181">Click **Security**.</span></span>
   
    <span data-ttu-id="db20a-182">![Zabezpečení](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="db20a-182">![Security](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "Security")</span></span>

11. <span data-ttu-id="db20a-183">Na **jednotné přihlašování** konfigurace proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db20a-183">On the **Single Sign-On** configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="db20a-184">![Jednotné přihlašování](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="db20a-184">![Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="db20a-185">a.</span><span class="sxs-lookup"><span data-stu-id="db20a-185">a.</span></span> <span data-ttu-id="db20a-186">Vyberte **povolit SAML**.</span><span class="sxs-lookup"><span data-stu-id="db20a-186">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="db20a-187">b.</span><span class="sxs-lookup"><span data-stu-id="db20a-187">b.</span></span> <span data-ttu-id="db20a-188">Klikněte na tlačítko **ruční konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="db20a-188">Click **Manual Configuration**.</span></span>
   
    <span data-ttu-id="db20a-189">c.</span><span class="sxs-lookup"><span data-stu-id="db20a-189">c.</span></span> <span data-ttu-id="db20a-190">V **SAML Post URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="db20a-190">In **SAML Post URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal</span></span>
   
    <span data-ttu-id="db20a-191">d.</span><span class="sxs-lookup"><span data-stu-id="db20a-191">d.</span></span> <span data-ttu-id="db20a-192">V **otisků certifikátu SAML** textovému poli, vložte hodnotu **kryptografický otisk** který jste zkopírovali z **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="db20a-192">In **SAML Certificate Fingerprint** textbox, paste the value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span>
  
    <span data-ttu-id="db20a-193">e.</span><span class="sxs-lookup"><span data-stu-id="db20a-193">e.</span></span> <span data-ttu-id="db20a-194">Kopírování **vaše ID účtu** a vložte hodnotu v **hodnota atributu** textového pole pod **přidat atribut** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="db20a-194">Copy **Your Account ID** value and paste the value in **Attribute Value** textbox under **Add Attribute** section in Azure portal.</span></span>
   
    <span data-ttu-id="db20a-195">f.</span><span class="sxs-lookup"><span data-stu-id="db20a-195">f.</span></span> <span data-ttu-id="db20a-196">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="db20a-196">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="db20a-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="db20a-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="db20a-198">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="db20a-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="db20a-199">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="db20a-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="db20a-200">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="db20a-200">Create an Azure AD test user</span></span>
<span data-ttu-id="db20a-201">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db20a-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="db20a-203">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db20a-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="db20a-204">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="db20a-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="db20a-206">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="db20a-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![<span data-ttu-id="db20a-207">Uživatelé a skupiny -> všichni uživatelé</span><span class="sxs-lookup"><span data-stu-id="db20a-207">Users and groups -> All users</span></span> ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="db20a-208">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db20a-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Uživatel](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="db20a-210">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db20a-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="db20a-212">a.</span><span class="sxs-lookup"><span data-stu-id="db20a-212">a.</span></span> <span data-ttu-id="db20a-213">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="db20a-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="db20a-214">b.</span><span class="sxs-lookup"><span data-stu-id="db20a-214">b.</span></span> <span data-ttu-id="db20a-215">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="db20a-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="db20a-216">c.</span><span class="sxs-lookup"><span data-stu-id="db20a-216">c.</span></span> <span data-ttu-id="db20a-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="db20a-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="db20a-218">d.</span><span class="sxs-lookup"><span data-stu-id="db20a-218">d.</span></span> <span data-ttu-id="db20a-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="db20a-219">Click **Create**.</span></span>
 
### <a name="create-a-tinfoil-security-test-user"></a><span data-ttu-id="db20a-220">Vytvoření zkušebního uživatele TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="db20a-220">Create a TINFOIL SECURITY test user</span></span>

<span data-ttu-id="db20a-221">Pokud chcete povolit uživatelům Azure AD přihlášení do TINFOIL SECURITY, musí být zřízená do TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="db20a-221">In order to enable Azure AD users to log into TINFOIL SECURITY, they must be provisioned into TINFOIL SECURITY.</span></span> <span data-ttu-id="db20a-222">V případě TINFOIL SECURITY zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="db20a-222">In the case of TINFOIL SECURITY, provisioning is a manual task.</span></span>

<span data-ttu-id="db20a-223">**Pokud chcete získat uživatele zřízený, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db20a-223">**To get a user provisioned, perform the following steps:**</span></span>

1. <span data-ttu-id="db20a-224">Pokud uživatel je součástí účet podnikové sítě, budete muset [kontaktujte tým podpory TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) se získat uživatelský účet vytvořený.</span><span class="sxs-lookup"><span data-stu-id="db20a-224">If the user is a part of an Enterprise account, you need to [contact the TINFOIL SECURITY support team](https://www.tinfoilsecurity.com/contact) to get the user account created.</span></span>

2. <span data-ttu-id="db20a-225">Pokud je uživatel běžný uživatel TINFOIL SECURITY SaaS, pak uživatel může přidávat spolupracovníka pro žádného uživatele lokalit.</span><span class="sxs-lookup"><span data-stu-id="db20a-225">If the user is a regular TINFOIL SECURITY SaaS user, then the user can add a collaborator to any of the user’s sites.</span></span> <span data-ttu-id="db20a-226">Tím se spustí proces k odeslání pozvánky k zadané e-mailu k vytvoření nového uživatelského účtu TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="db20a-226">This triggers a process to send an invitation to the specified email to create a new TINFOIL SECURITY user account.</span></span>

> [!NOTE]
> <span data-ttu-id="db20a-227">Další nástroje pro tvorbu TINFOIL SECURITY uživatelského účtu nebo rozhraní API poskytovaných TINFOIL SECURITY můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db20a-227">You can use any other TINFOIL SECURITY user account creation tools or APIs provided by TINFOIL SECURITY to provision Azure AD user accounts.</span></span>
> 
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="db20a-228">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="db20a-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="db20a-229">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="db20a-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TINFOIL SECURITY.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="db20a-231">**Pokud chcete přiřadit Britta Simon TINFOIL SECURITY, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="db20a-231">**To assign Britta Simon to TINFOIL SECURITY, perform the following steps:**</span></span>

1. <span data-ttu-id="db20a-232">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="db20a-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="db20a-234">V seznamu aplikací vyberte **TINFOIL SECURITY**.</span><span class="sxs-lookup"><span data-stu-id="db20a-234">In the applications list, select **TINFOIL SECURITY**.</span></span>

    ![Vyberte TINFOIL SECURITY](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. <span data-ttu-id="db20a-236">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="db20a-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="db20a-238">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="db20a-238">Click **Add** button.</span></span> <span data-ttu-id="db20a-239">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db20a-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="db20a-241">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="db20a-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="db20a-242">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db20a-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="db20a-243">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="db20a-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="db20a-244">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="db20a-244">Test single sign-on</span></span>

<span data-ttu-id="db20a-245">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="db20a-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="db20a-246">Když kliknete na dlaždici TINFOIL SECURITY na přístupovém panelu, jste měli získat automaticky přihlášení k aplikace TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="db20a-246">When you click the TINFOIL SECURITY tile in the Access Panel, you should get automatically signed-on to your TINFOIL SECURITY application.</span></span> <span data-ttu-id="db20a-247">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="db20a-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="db20a-248">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="db20a-248">Additional resources</span></span>

* [<span data-ttu-id="db20a-249">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db20a-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="db20a-250">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="db20a-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

