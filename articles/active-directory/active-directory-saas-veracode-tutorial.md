---
title: 'Kurz: Azure Active Directory integrace s Veracode | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Veracode."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d49349c5ae08e67d91e30967f3644623211823ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a><span data-ttu-id="302f1-103">Kurz: Azure Active Directory integrace s Veracode</span><span class="sxs-lookup"><span data-stu-id="302f1-103">Tutorial: Azure Active Directory integration with Veracode</span></span>

<span data-ttu-id="302f1-104">V tomto kurzu zjistěte, jak integrovat Veracode s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="302f1-104">In this tutorial, you learn how to integrate Veracode with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="302f1-105">Integrace Veracode s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="302f1-105">Integrating Veracode with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="302f1-106">Můžete ovládat ve službě Azure AD, který má přístup k Veracode.</span><span class="sxs-lookup"><span data-stu-id="302f1-106">You can control in Azure AD who has access to Veracode.</span></span>
- <span data-ttu-id="302f1-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Veracode (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="302f1-107">You can enable your users to automatically get signed-on to Veracode (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="302f1-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="302f1-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="302f1-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="302f1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="302f1-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="302f1-110">Prerequisites</span></span>

<span data-ttu-id="302f1-111">Konfigurace integrace Azure AD s Veracode, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="302f1-111">To configure Azure AD integration with Veracode, you need the following items:</span></span>

- <span data-ttu-id="302f1-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="302f1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="302f1-113">Veracode jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="302f1-113">A Veracode single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="302f1-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="302f1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="302f1-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="302f1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="302f1-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="302f1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="302f1-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="302f1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="302f1-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="302f1-118">Scenario description</span></span>
<span data-ttu-id="302f1-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="302f1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="302f1-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="302f1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="302f1-121">Přidat Veracode z Galerie</span><span class="sxs-lookup"><span data-stu-id="302f1-121">Add Veracode from the gallery</span></span>
2. <span data-ttu-id="302f1-122">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="302f1-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-veracode-from-the-gallery"></a><span data-ttu-id="302f1-123">Přidat Veracode z Galerie</span><span class="sxs-lookup"><span data-stu-id="302f1-123">Add Veracode from the gallery</span></span>
<span data-ttu-id="302f1-124">Při konfiguraci integrace Veracode do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Veracode z galerie.</span><span class="sxs-lookup"><span data-stu-id="302f1-124">To configure the integration of Veracode into Azure AD, you need to add Veracode from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="302f1-125">**Pokud chcete přidat Veracode z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="302f1-125">**To add Veracode from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="302f1-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="302f1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="302f1-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="302f1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="302f1-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="302f1-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="302f1-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="302f1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="302f1-133">Do vyhledávacího pole zadejte **Veracode**, vyberte **Veracode** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="302f1-133">In the search box, type **Veracode**, select  **Veracode** from result panel then click **Add** button to add the application.</span></span>

    ![Veracode v seznamu výsledků](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="302f1-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="302f1-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="302f1-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Veracode podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="302f1-136">In this section, you configure and test Azure AD single sign-on with Veracode based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="302f1-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Veracode je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="302f1-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Veracode is to a user in Azure AD.</span></span> <span data-ttu-id="302f1-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Veracode musí navázat.</span><span class="sxs-lookup"><span data-stu-id="302f1-138">In other words, a link relationship between an Azure AD user and the related user in Veracode needs to be established.</span></span>

<span data-ttu-id="302f1-139">V Veracode, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="302f1-139">In Veracode, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="302f1-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Veracode, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="302f1-140">To configure and test Azure AD single sign-on with Veracode, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="302f1-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="302f1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="302f1-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="302f1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="302f1-143">**[Vytvoření zkušebního uživatele Veracode](#create-a-veracode-test-user)**  – Pokud chcete mít protějšek Britta Simon v Veracode propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="302f1-143">**[Create a Veracode test user](#create-a-veracode-test-user)** - to have a counterpart of Britta Simon in Veracode that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="302f1-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="302f1-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="302f1-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="302f1-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="302f1-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="302f1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="302f1-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Veracode.</span><span class="sxs-lookup"><span data-stu-id="302f1-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Veracode application.</span></span>

<span data-ttu-id="302f1-148">**Ke konfiguraci Azure AD jednotné přihlašování s Veracode, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="302f1-148">**To configure Azure AD single sign-on with Veracode, perform the following steps:**</span></span>

1. <span data-ttu-id="302f1-149">Na portálu Azure na **Veracode** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="302f1-149">In the Azure portal, on the **Veracode** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="302f1-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="302f1-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. <span data-ttu-id="302f1-153">Na **Veracode domény a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="302f1-153">On the **Veracode Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. <span data-ttu-id="302f1-155">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="302f1-155">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. <span data-ttu-id="302f1-157">Cílem této části se popisují, jak uživatelům povolit ověřování na Veracode ke svému účtu ve službě Azure AD využívající federaci na základě protokolu SAML.</span><span class="sxs-lookup"><span data-stu-id="302f1-157">The objective of this section is to outline how to enable users to authenticate to Veracode with their account in Azure AD using federation based on the SAML protocol.</span></span>

    <span data-ttu-id="302f1-158">Aplikace Veracode očekává SAML kontrolní výrazy ve specifickém formátu, který můžete přidat mapování vlastní atribut vyžaduje vaše **atributy tokenu saml** konfigurace.</span><span class="sxs-lookup"><span data-stu-id="302f1-158">Your Veracode application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration.</span></span> <span data-ttu-id="302f1-159">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="302f1-159">The following screenshot shows an example for this.</span></span>
    
    <span data-ttu-id="302f1-160">![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="302f1-160">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "Attributes")</span></span>

6. <span data-ttu-id="302f1-161">Pokud chcete přidat mapování požadovaný atribut, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="302f1-161">To add the required attribute mappings, perform the following steps:</span></span>

    | <span data-ttu-id="302f1-162">Název atributu</span><span class="sxs-lookup"><span data-stu-id="302f1-162">Attribute Name</span></span> | <span data-ttu-id="302f1-163">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="302f1-163">Attribute Value</span></span> |
    |--- |--- |
    | <span data-ttu-id="302f1-164">FirstName</span><span class="sxs-lookup"><span data-stu-id="302f1-164">firstname</span></span> |<span data-ttu-id="302f1-165">User.givenName</span><span class="sxs-lookup"><span data-stu-id="302f1-165">User.givenname</span></span> |
    | <span data-ttu-id="302f1-166">Příjmení</span><span class="sxs-lookup"><span data-stu-id="302f1-166">lastname</span></span> |<span data-ttu-id="302f1-167">User.Surname</span><span class="sxs-lookup"><span data-stu-id="302f1-167">User.surname</span></span> |
    | <span data-ttu-id="302f1-168">E-mailu</span><span class="sxs-lookup"><span data-stu-id="302f1-168">email</span></span> |<span data-ttu-id="302f1-169">User.Mail</span><span class="sxs-lookup"><span data-stu-id="302f1-169">User.mail</span></span> |
    
    <span data-ttu-id="302f1-170">a.</span><span class="sxs-lookup"><span data-stu-id="302f1-170">a.</span></span> <span data-ttu-id="302f1-171">Pro každý řádek dat v předchozí tabulce, klikněte na tlačítko **přidat atribut uživatele**.</span><span class="sxs-lookup"><span data-stu-id="302f1-171">For each data row in the table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="302f1-172">![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="302f1-172">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "Attributes")</span></span>
    
    <span data-ttu-id="302f1-173">![Atributy](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "atributy")</span><span class="sxs-lookup"><span data-stu-id="302f1-173">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "Attributes")</span></span>
    
    <span data-ttu-id="302f1-174">b.</span><span class="sxs-lookup"><span data-stu-id="302f1-174">b.</span></span> <span data-ttu-id="302f1-175">V **název atributu** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="302f1-175">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="302f1-176">c.</span><span class="sxs-lookup"><span data-stu-id="302f1-176">c.</span></span> <span data-ttu-id="302f1-177">V **hodnota atributu** textovému poli, vyberte hodnotu atributu zobrazený pro tento řádek.</span><span class="sxs-lookup"><span data-stu-id="302f1-177">In the **Attribute Value** textbox, select the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="302f1-178">d.</span><span class="sxs-lookup"><span data-stu-id="302f1-178">d.</span></span> <span data-ttu-id="302f1-179">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="302f1-179">Click **Ok**.</span></span>

7. <span data-ttu-id="302f1-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="302f1-180">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="302f1-182">Na **Veracode konfigurace** klikněte na tlačítko **konfigurace Veracode** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="302f1-182">On the **Veracode Configuration** section, click **Configure Veracode** to open **Configure sign-on** window.</span></span> <span data-ttu-id="302f1-183">Kopírování **SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="302f1-183">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurace Veracode](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. <span data-ttu-id="302f1-185">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Veracode.</span><span class="sxs-lookup"><span data-stu-id="302f1-185">In a different web browser window, log into your Veracode company site as an administrator.</span></span>

10. <span data-ttu-id="302f1-186">V nabídce v horní části, klikněte na tlačítko **nastavení**a potom klikněte na **správce**.</span><span class="sxs-lookup"><span data-stu-id="302f1-186">In the menu on the top, click **Settings**, and then click **Admin**.</span></span>
   
    <span data-ttu-id="302f1-187">![Správa](./media/active-directory-saas-veracode-tutorial/ic802911.png "správy")</span><span class="sxs-lookup"><span data-stu-id="302f1-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span></span>

11. <span data-ttu-id="302f1-188">Klikněte **SAML** kartě.</span><span class="sxs-lookup"><span data-stu-id="302f1-188">Click the **SAML** tab.</span></span>

12. <span data-ttu-id="302f1-189">V **organizace SAML nastavení** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="302f1-189">In the **Organization SAML Settings** section, perform the following steps:</span></span>
   
    <span data-ttu-id="302f1-190">![Správa](./media/active-directory-saas-veracode-tutorial/ic802912.png "správy")</span><span class="sxs-lookup"><span data-stu-id="302f1-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span></span>
   
    <span data-ttu-id="302f1-191">a.</span><span class="sxs-lookup"><span data-stu-id="302f1-191">a.</span></span>  <span data-ttu-id="302f1-192">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="302f1-192">In  **Issuer** textbox, paste the value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="302f1-193">b.</span><span class="sxs-lookup"><span data-stu-id="302f1-193">b.</span></span> <span data-ttu-id="302f1-194">Chcete-li nahrát svůj certifikát stažený z portálu Azure, klikněte na tlačítko **zvolit soubor**.</span><span class="sxs-lookup"><span data-stu-id="302f1-194">To upload your downloaded certificate from Azure portal, click **Choose File**.</span></span>
   
    <span data-ttu-id="302f1-195">c.</span><span class="sxs-lookup"><span data-stu-id="302f1-195">c.</span></span> <span data-ttu-id="302f1-196">Vyberte **povolit samoobslužné registrace**.</span><span class="sxs-lookup"><span data-stu-id="302f1-196">Select **Enable Self Registration**.</span></span>

13. <span data-ttu-id="302f1-197">V **nastavení samoobslužné registrace** část, proveďte následující kroky a pak klikněte na tlačítko **Uložit**:</span><span class="sxs-lookup"><span data-stu-id="302f1-197">In the **Self Registration Settings** section, perform the following steps, and then click **Save**:</span></span>
   
    <span data-ttu-id="302f1-198">![Správa](./media/active-directory-saas-veracode-tutorial/ic802913.png "správy")</span><span class="sxs-lookup"><span data-stu-id="302f1-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span></span>
   
    <span data-ttu-id="302f1-199">a.</span><span class="sxs-lookup"><span data-stu-id="302f1-199">a.</span></span> <span data-ttu-id="302f1-200">Jako **nové aktivace uživatele**, vyberte **vyžaduje její aktivace ne**.</span><span class="sxs-lookup"><span data-stu-id="302f1-200">As **New User Activation**, select **No Activation Required**.</span></span>
   
    <span data-ttu-id="302f1-201">b.</span><span class="sxs-lookup"><span data-stu-id="302f1-201">b.</span></span> <span data-ttu-id="302f1-202">Jako **aktualizace dat uživatele**, vyberte **předvoleb Veracode uživatelská Data**.</span><span class="sxs-lookup"><span data-stu-id="302f1-202">As **User Data Updates**, select **Preference Veracode User Data**.</span></span>
   
    <span data-ttu-id="302f1-203">c.</span><span class="sxs-lookup"><span data-stu-id="302f1-203">c.</span></span> <span data-ttu-id="302f1-204">Pro **SAML atribut podrobnosti**, vyberte následující:</span><span class="sxs-lookup"><span data-stu-id="302f1-204">For **SAML Attribute Details**, select the following:</span></span>
      * <span data-ttu-id="302f1-205">**Role uživatele**</span><span class="sxs-lookup"><span data-stu-id="302f1-205">**User Roles**</span></span>
      * <span data-ttu-id="302f1-206">**Správce zásad**</span><span class="sxs-lookup"><span data-stu-id="302f1-206">**Policy Administrator**</span></span>
      * <span data-ttu-id="302f1-207">**Kontrolora**</span><span class="sxs-lookup"><span data-stu-id="302f1-207">**Reviewer**</span></span>
      * <span data-ttu-id="302f1-208">**Realizace zabezpečení**</span><span class="sxs-lookup"><span data-stu-id="302f1-208">**Security Lead**</span></span>
      * <span data-ttu-id="302f1-209">**Vedení**</span><span class="sxs-lookup"><span data-stu-id="302f1-209">**Executive**</span></span>
      * <span data-ttu-id="302f1-210">**Odesílatel**</span><span class="sxs-lookup"><span data-stu-id="302f1-210">**Submitter**</span></span>
      * <span data-ttu-id="302f1-211">**Tvůrce**</span><span class="sxs-lookup"><span data-stu-id="302f1-211">**Creator**</span></span>
      * <span data-ttu-id="302f1-212">**Všechny kontroly typy**</span><span class="sxs-lookup"><span data-stu-id="302f1-212">**All Scan Types**</span></span>
      * <span data-ttu-id="302f1-213">**Členství v týmu**</span><span class="sxs-lookup"><span data-stu-id="302f1-213">**Team Memberships**</span></span>
      * <span data-ttu-id="302f1-214">**Výchozí tým**</span><span class="sxs-lookup"><span data-stu-id="302f1-214">**Default Team**</span></span>

> [!TIP]
> <span data-ttu-id="302f1-215">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="302f1-215">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="302f1-216">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="302f1-216">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="302f1-217">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="302f1-217">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="302f1-218">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="302f1-218">Create an Azure AD test user</span></span>

<span data-ttu-id="302f1-219">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="302f1-219">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="302f1-221">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="302f1-221">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="302f1-222">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="302f1-222">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="302f1-224">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="302f1-224">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="302f1-226">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="302f1-226">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="302f1-228">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="302f1-228">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    <span data-ttu-id="302f1-230">a.</span><span class="sxs-lookup"><span data-stu-id="302f1-230">a.</span></span> <span data-ttu-id="302f1-231">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="302f1-231">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="302f1-232">b.</span><span class="sxs-lookup"><span data-stu-id="302f1-232">b.</span></span> <span data-ttu-id="302f1-233">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="302f1-233">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="302f1-234">c.</span><span class="sxs-lookup"><span data-stu-id="302f1-234">c.</span></span> <span data-ttu-id="302f1-235">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="302f1-235">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="302f1-236">d.</span><span class="sxs-lookup"><span data-stu-id="302f1-236">d.</span></span> <span data-ttu-id="302f1-237">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="302f1-237">Click **Create**.</span></span>
 
### <a name="create-a-veracode-test-user"></a><span data-ttu-id="302f1-238">Vytvoření zkušebního uživatele Veracode</span><span class="sxs-lookup"><span data-stu-id="302f1-238">Create a Veracode test user</span></span>
<span data-ttu-id="302f1-239">Pokud chcete povolit uživatelům Azure AD přihlášení do Veracode, musí být zřízená do Veracode.</span><span class="sxs-lookup"><span data-stu-id="302f1-239">In order to enable Azure AD users to log into Veracode, they must be provisioned into Veracode.</span></span> <span data-ttu-id="302f1-240">V případě Veracode zřizování je automatizaci úloh.</span><span class="sxs-lookup"><span data-stu-id="302f1-240">In the case of Veracode, provisioning is an automated task.</span></span> <span data-ttu-id="302f1-241">Neexistuje žádná položka akce za vás.</span><span class="sxs-lookup"><span data-stu-id="302f1-241">There is no action item for you.</span></span> <span data-ttu-id="302f1-242">Uživatelé jsou automaticky vytvářeny v případě potřeby při prvním jeden přihlašování pokusu.</span><span class="sxs-lookup"><span data-stu-id="302f1-242">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

> [!NOTE]
> <span data-ttu-id="302f1-243">Můžete použít všechny ostatní Veracode uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Veracode ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="302f1-243">You can use any other Veracode user account creation tools or APIs provided by Veracode to provision Azure AD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="302f1-244">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="302f1-244">Assign the Azure AD test user</span></span>

<span data-ttu-id="302f1-245">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Veracode.</span><span class="sxs-lookup"><span data-stu-id="302f1-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Veracode.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="302f1-247">**Pokud chcete přiřadit Britta Simon Veracode, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="302f1-247">**To assign Britta Simon to Veracode, perform the following steps:**</span></span>

1. <span data-ttu-id="302f1-248">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="302f1-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="302f1-250">V seznamu aplikací vyberte **Veracode**.</span><span class="sxs-lookup"><span data-stu-id="302f1-250">In the applications list, select **Veracode**.</span></span>

    ![V seznamu aplikací na Veracode odkaz](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. <span data-ttu-id="302f1-252">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="302f1-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="302f1-254">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="302f1-254">Click **Add** button.</span></span> <span data-ttu-id="302f1-255">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="302f1-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="302f1-257">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="302f1-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="302f1-258">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="302f1-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="302f1-259">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="302f1-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="302f1-260">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="302f1-260">Test single sign-on</span></span>

<span data-ttu-id="302f1-261">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="302f1-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="302f1-262">Když kliknete na dlaždici Veracode na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Veracode.</span><span class="sxs-lookup"><span data-stu-id="302f1-262">When you click the Veracode tile in the Access Panel, you should get automatically signed-on to your Veracode application.</span></span>
<span data-ttu-id="302f1-263">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="302f1-263">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="302f1-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="302f1-264">Additional resources</span></span>

* [<span data-ttu-id="302f1-265">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="302f1-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="302f1-266">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="302f1-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

