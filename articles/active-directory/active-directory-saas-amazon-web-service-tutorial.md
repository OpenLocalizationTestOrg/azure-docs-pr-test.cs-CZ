---
title: 'Kurz: Azure Active Directory integrace s Amazon Web Services (AWS) | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 0fb9c8f428368271b548e3f174726fa01ea910c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a><span data-ttu-id="cf0b6-103">Kurz: Azure Active Directory integrace s Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="cf0b6-103">Tutorial: Azure Active Directory integration with Amazon Web Services (AWS)</span></span>

<span data-ttu-id="cf0b6-104">V tomto kurzu zjistěte, jak integrovat Amazon Web Services (AWS) s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-104">In this tutorial, you learn how to integrate Amazon Web Services (AWS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cf0b6-105">Integrace Amazon Web Services (AWS) s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-105">Integrating Amazon Web Services (AWS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cf0b6-106">Můžete řídit ve službě Azure AD, který má přístup k Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="cf0b6-106">You can control in Azure AD who has access to Amazon Web Services (AWS)</span></span>
- <span data-ttu-id="cf0b6-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k Amazon Web Services (AWS) (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf0b6-107">You can enable your users to automatically get signed-on to Amazon Web Services (AWS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cf0b6-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cf0b6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cf0b6-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="cf0b6-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cf0b6-110">Prerequisites</span></span>

<span data-ttu-id="cf0b6-111">Ke konfiguraci integrace služby Azure AD pomocí Amazon Web Services (AWS), potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-111">To configure Azure AD integration with Amazon Web Services (AWS), you need the following items:</span></span>

- <span data-ttu-id="cf0b6-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf0b6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cf0b6-113">Amazon Web Services (AWS) jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cf0b6-113">Amazon Web Services (AWS) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cf0b6-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cf0b6-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cf0b6-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cf0b6-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cf0b6-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cf0b6-118">Scenario description</span></span>
<span data-ttu-id="cf0b6-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cf0b6-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cf0b6-121">Přidání Amazon Web Services (AWS) z Galerie</span><span class="sxs-lookup"><span data-stu-id="cf0b6-121">Adding Amazon Web Services (AWS) from the gallery</span></span>
2. <span data-ttu-id="cf0b6-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf0b6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a><span data-ttu-id="cf0b6-123">Přidání Amazon Web Services (AWS) z Galerie</span><span class="sxs-lookup"><span data-stu-id="cf0b6-123">Adding Amazon Web Services (AWS) from the gallery</span></span>
<span data-ttu-id="cf0b6-124">Při konfiguraci integrace služby Amazon Web Services (AWS) do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS Amazon Web Services (AWS) z galerie.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-124">To configure the integration of Amazon Web Services (AWS) into Azure AD, you need to add Amazon Web Services (AWS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cf0b6-125">**Pokud chcete přidat Amazon Web Services (AWS) z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-125">**To add Amazon Web Services (AWS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cf0b6-126">V  **[portálu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cf0b6-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cf0b6-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cf0b6-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cf0b6-133">Do vyhledávacího pole zadejte **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-133">In the search box, type **Amazon Web Services (AWS)**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. <span data-ttu-id="cf0b6-135">Na panelu výsledků vyberte **Amazon Web Services (AWS)**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-135">In the results panel, select **Amazon Web Services (AWS)**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cf0b6-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf0b6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cf0b6-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cf0b6-138">In this section, you configure and test Azure AD single sign-on with Amazon Web Services (AWS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cf0b6-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Amazon Web Services (AWS) je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Amazon Web Services (AWS) is to a user in Azure AD.</span></span> <span data-ttu-id="cf0b6-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-140">In other words, a link relationship between an Azure AD user and the related user in Amazon Web Services (AWS) needs to be established.</span></span>

<span data-ttu-id="cf0b6-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Amazon Web Services (AWS).</span></span>

<span data-ttu-id="cf0b6-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-142">To configure and test Azure AD single sign-on with Amazon Web Services (AWS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cf0b6-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cf0b6-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cf0b6-145">**[Vytváření testovacího uživatele Amazon Web Services](#creating-an-amazon-web-services-test-user)**  – Pokud chcete mít v Amazon Web Services (AWS), propojené služby Azure AD reprezentace jí protějšek Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-145">**[Creating an Amazon Web Services test user](#creating-an-amazon-web-services-test-user)** - to have a counterpart of Britta Simon in Amazon Web Services (AWS) that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="cf0b6-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cf0b6-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cf0b6-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf0b6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cf0b6-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Amazon Web Services (AWS) application.</span></span>

<span data-ttu-id="cf0b6-150">**Pokud chcete konfigurovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-150">**To configure Azure AD single sign-on with Amazon Web Services (AWS), perform the following steps:**</span></span>

1. <span data-ttu-id="cf0b6-151">Na portálu Azure na **Amazon Web Services (AWS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-151">In the Azure Portal, on the **Amazon Web Services (AWS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cf0b6-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. <span data-ttu-id="cf0b6-155">Na **Amazon Web Services (AWS) domény a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-155">On the **Amazon Web Services (AWS) Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. <span data-ttu-id="cf0b6-157">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. <span data-ttu-id="cf0b6-159">Aplikace Software Amazon Web Services (AWS) očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-159">The Amazon Web Services (AWS) Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="cf0b6-160">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-160">Please configure the following claims for this application.</span></span> <span data-ttu-id="cf0b6-161">Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-161">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="cf0b6-162">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-162">The following screenshot shows an example for this.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. <span data-ttu-id="cf0b6-164">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-164">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="cf0b6-165">Název atributu</span><span class="sxs-lookup"><span data-stu-id="cf0b6-165">Attribute Name</span></span>  | <span data-ttu-id="cf0b6-166">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="cf0b6-166">Attribute Value</span></span> | <span data-ttu-id="cf0b6-167">obor názvů</span><span class="sxs-lookup"><span data-stu-id="cf0b6-167">Namespace</span></span> |
    | --------------- | --------------- | --------------- |
    | <span data-ttu-id="cf0b6-168">RoleSessionName</span><span class="sxs-lookup"><span data-stu-id="cf0b6-168">RoleSessionName</span></span> | <span data-ttu-id="cf0b6-169">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="cf0b6-169">user.userprincipalname</span></span> | <span data-ttu-id="cf0b6-170">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="cf0b6-170">https://aws.amazon.com/SAML/Attributes</span></span> |
    | <span data-ttu-id="cf0b6-171">Role</span><span class="sxs-lookup"><span data-stu-id="cf0b6-171">Role</span></span>            | <span data-ttu-id="cf0b6-172">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="cf0b6-172">user.assignedroles</span></span> |  <span data-ttu-id="cf0b6-173">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="cf0b6-173">https://aws.amazon.com/SAML/Attributes</span></span> |
    
    >[!TIP]
    ><span data-ttu-id="cf0b6-174">Musíte nakonfigurovat zřizování uživatelů ve službě Azure AD se načíst všechny role z konzoly AWS.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-174">You need to configure the user provisioning in Azure AD to fetch all the roles from AWS Console.</span></span> <span data-ttu-id="cf0b6-175">Naleznete v následujících zřizování kroků.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-175">Please refer the provisioning steps below.</span></span>

    <span data-ttu-id="cf0b6-176">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-176">a.</span></span> <span data-ttu-id="cf0b6-177">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="cf0b6-179">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-179">b.</span></span> <span data-ttu-id="cf0b6-180">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="cf0b6-182">c.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-182">c.</span></span> <span data-ttu-id="cf0b6-183">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-183">From the **Value** list, type the attribute value shown for that row.</span></span> <span data-ttu-id="cf0b6-184">Přidejte hodnotu Namespace jako ve výše uvedených.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-184">Add the Namespace value as given above.</span></span>
    
    <span data-ttu-id="cf0b6-185">d.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-185">d.</span></span> <span data-ttu-id="cf0b6-186">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-186">Click **Ok**.</span></span>

7. <span data-ttu-id="cf0b6-187">Klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení v Azure.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-187">Click **Save** button to save the settings on Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="cf0b6-189">V okně jiný prohlížeč přihlašování k webu společnosti Amazon Web Services (AWS) jako správce.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-189">In a different browser window, sign-on to your Amazon Web Services (AWS) company site as administrator.</span></span>

9. <span data-ttu-id="cf0b6-190">Klikněte na tlačítko **konzoly domovské**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-190">Click **Console Home**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][11]

10. <span data-ttu-id="cf0b6-192">Klikněte na tlačítko **IAM** z **zabezpečení, Identity a dodržování předpisů** služby.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-192">Click **IAM** from **Security, Identity & Compliance** service.</span></span>
   
    ![Konfigurovat jednotné přihlašování][12]

11. <span data-ttu-id="cf0b6-194">Klikněte na tlačítko **zprostředkovatelů Identity**a potom klikněte na **vytvoření zprostředkovatele**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-194">Click **Identity Providers**, and then click **Create Provider**.</span></span>
   
    ![Konfigurovat jednotné přihlašování][13]

12. <span data-ttu-id="cf0b6-196">Na **konfigurace zprostředkovatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-196">On the **Configure Provider** dialog page, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování][14]
 
    <span data-ttu-id="cf0b6-198">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-198">a.</span></span> <span data-ttu-id="cf0b6-199">Jako **typ zprostředkovatele**, vyberte **SAML**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-199">As **Provider Type**, select **SAML**.</span></span>

    <span data-ttu-id="cf0b6-200">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-200">b.</span></span> <span data-ttu-id="cf0b6-201">V **název zprostředkovatele** textovému poli, zadejte název zprostředkovatele (např: *službou WAAD*).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-201">In the **Provider Name** textbox, type a provider name (e.g.: *WAAD*).</span></span>

    <span data-ttu-id="cf0b6-202">c.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-202">c.</span></span> <span data-ttu-id="cf0b6-203">Chcete-li nahrát soubor stažený metadata, klikněte na tlačítko **zvolit soubor**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-203">To upload your downloaded metadata file, click **Choose File**.</span></span>

    <span data-ttu-id="cf0b6-204">d.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-204">d.</span></span> <span data-ttu-id="cf0b6-205">Klikněte na tlačítko **dalším krokem**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-205">Click **Next Step**.</span></span>

13. <span data-ttu-id="cf0b6-206">Na **ověřte informace o poskytovateli** dialogové okno stránky, klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-206">On the **Verify Provider Information** dialog page, click **Create**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování][15]

14. <span data-ttu-id="cf0b6-208">Klikněte na tlačítko **role**a potom klikněte na **vytvořit novou roli**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-208">Click **Roles**, and then click **Create New Role**.</span></span> 
    
    ![Konfigurovat jednotné přihlašování][16]

15. <span data-ttu-id="cf0b6-210">Na **nastavit název Role** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-210">On the **Set Role Name** dialog, perform the following steps:</span></span> 
    
    ![Konfigurovat jednotné přihlašování][17] 

    <span data-ttu-id="cf0b6-212">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-212">a.</span></span> <span data-ttu-id="cf0b6-213">V **název Role** textovému poli, zadejte název role (např: *TestUser*).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-213">In the **Role Name** textbox, type a role name (e.g.: *TestUser*).</span></span> 

    <span data-ttu-id="cf0b6-214">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-214">b.</span></span> <span data-ttu-id="cf0b6-215">Klikněte na tlačítko **dalším krokem**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-215">Click **Next Step**.</span></span>

16. <span data-ttu-id="cf0b6-216">Na **vyberte typ Role** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-216">On the **Select Role Type** dialog, perform the following steps:</span></span> 
    
    ![Konfigurovat jednotné přihlašování][18] 

    <span data-ttu-id="cf0b6-218">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-218">a.</span></span> <span data-ttu-id="cf0b6-219">Vyberte **Role pro přístup poskytovatele Identity**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-219">Select **Role For Identity Provider Access**.</span></span> 

    <span data-ttu-id="cf0b6-220">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-220">b.</span></span> <span data-ttu-id="cf0b6-221">V **Grant webové jednotné přihlašování (WebSSO) přístup k poskytovatelům SAML** klikněte na tlačítko **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-221">In the **Grant Web Single Sign-On (WebSSO) access to SAML providers** section, click **Select**.</span></span>

17. <span data-ttu-id="cf0b6-222">Na **navázání vztahu důvěryhodnosti** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-222">On the **Establish Trust** dialog, perform the following steps:</span></span>  
    
    ![Konfigurovat jednotné přihlašování][19] 

    <span data-ttu-id="cf0b6-224">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-224">a.</span></span> <span data-ttu-id="cf0b6-225">Jako poskytovatel SAML, vyberte poskytovateli SAML, který jste vytvořili dříve (např: *službou WAAD*)</span><span class="sxs-lookup"><span data-stu-id="cf0b6-225">As SAML provider, select the SAML provider you have created previously (e.g.: *WAAD*)</span></span>
  
    <span data-ttu-id="cf0b6-226">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-226">b.</span></span> <span data-ttu-id="cf0b6-227">Klikněte na tlačítko **dalším krokem**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-227">Click **Next Step**.</span></span>

18. <span data-ttu-id="cf0b6-228">Na **ověřte důvěřovat Role** dialogové okno, klikněte na tlačítko **další krok**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-228">On the **Verify Role Trust** dialog, click **Next Step**.</span></span>
    
    ![Konfigurovat jednotné přihlašování][32]

19. <span data-ttu-id="cf0b6-230">Na **připojit zásady** dialogové okno, klikněte na tlačítko **další krok**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-230">On the **Attach Policy** dialog, click **Next Step**.</span></span>
    
    ![Konfigurovat jednotné přihlašování][33]

20. <span data-ttu-id="cf0b6-232">Na **zkontrolujte** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-232">On the **Review** dialog, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování][34]
 
    <span data-ttu-id="cf0b6-234">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-234">a.</span></span> <span data-ttu-id="cf0b6-235">Klikněte na tlačítko **vytvořit roli**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-235">Click **Create Role**.</span></span>

    <span data-ttu-id="cf0b6-236">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-236">b.</span></span> <span data-ttu-id="cf0b6-237">Vytvořit tolik role podle potřeby a jejich namapování na zprostředkovatele Identity.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-237">Create as many roles as needed and map them to the Identity Provider.</span></span>

21. <span data-ttu-id="cf0b6-238">Teď nakonfigurujte zřizování načíst všechny role z AWS uživatelů</span><span class="sxs-lookup"><span data-stu-id="cf0b6-238">Now configure the user provisioning to fetch all the roles from AWS</span></span>

    <span data-ttu-id="cf0b6-239">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-239">a.</span></span> <span data-ttu-id="cf0b6-240">V konzole AWS přihlášení pomocí kořenového účtu</span><span class="sxs-lookup"><span data-stu-id="cf0b6-240">In the AWS Console login with your root account</span></span>

    <span data-ttu-id="cf0b6-241">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-241">b.</span></span> <span data-ttu-id="cf0b6-242">V pravém horním rohu klikněte na své jméno a potom klikněte na **Moje zabezpečovací pověření** možnost.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-242">In the top right corner click your name and then click the **My Security Credentials** option.</span></span> <span data-ttu-id="cf0b6-243">Tím se otevře na obrazovce jako upozornění.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-243">This will open up a screen as a warning message.</span></span> <span data-ttu-id="cf0b6-244">Klikněte na tlačítko **zabezpečovací pověření** tlačítko předat na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-244">Click the button **Security Credentials** button to pass the screen.</span></span>
        
       ![Konfigurovat jednotné přihlašování][36]

       ![Konfigurovat jednotné přihlašování][37]

    <span data-ttu-id="cf0b6-247">c.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-247">c.</span></span> <span data-ttu-id="cf0b6-248">V části přístupových klíčů klepněte **vytvořit nový přístupový klíč** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-248">In the Access Keys section click the **Create New Access Key** button.</span></span> <span data-ttu-id="cf0b6-249">To vygeneruje ID přístupu klíč a hodnotu tokenu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-249">This generates the Access Key ID and a token value.</span></span>
    
       ![Konfigurovat jednotné přihlašování][38]

    <span data-ttu-id="cf0b6-251">d.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-251">d.</span></span> <span data-ttu-id="cf0b6-252">Zkopírujte oba tyto hodnoty a také, tak, aby neztratili ho stáhnout.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-252">Copy both these values and also download it, so that you don't lose it.</span></span>

    <span data-ttu-id="cf0b6-253">e.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-253">e.</span></span> <span data-ttu-id="cf0b6-254">Na portálu Azure, na stránce integrace aplikace Amazon Web Services (AWS), klikněte na tlačítko **zřizování**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-254">In the Azure Portal, on the Amazon Web Services (AWS) application integration page, click **Provisioning**.</span></span>
        
       ![Konfigurovat jednotné přihlašování][35]

    <span data-ttu-id="cf0b6-256">f.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-256">f.</span></span> <span data-ttu-id="cf0b6-257">Nastavit režim zřizování **automatické**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-257">Set the Provisioning mode to **Automatic**</span></span>
        
       ![Konfigurovat jednotné přihlašování][39]

    <span data-ttu-id="cf0b6-259">g.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-259">g.</span></span> <span data-ttu-id="cf0b6-260">Nyní ve **clientsecret** a **tajný klíč tokenu** vložte odpovídající hodnoty, které jste zkopírovali z konzoly AWS.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-260">Now in the **clientsecret** and **Secret Token** paste the corresponding values, which you have copied from AWS Console.</span></span>
    
    <span data-ttu-id="cf0b6-261">h.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-261">h.</span></span> <span data-ttu-id="cf0b6-262">Můžete kliknout na **otestovat připojení** tlačítko Testovat připojení.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-262">You can click the **Test Connection** button to test the connectivity.</span></span> <span data-ttu-id="cf0b6-263">Po úspěšné, můžete začít konektor zřizování.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-263">Once that is successful then you can start the provisioning connector.</span></span>
       
       ![Konfigurovat jednotné přihlašování][40]

    <span data-ttu-id="cf0b6-265">i.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-265">i.</span></span> <span data-ttu-id="cf0b6-266">Teď povolit stav zřizování na **na**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-266">Now enable the Provisioning Status to **On**.</span></span> <span data-ttu-id="cf0b6-267">Tím se spustí načítání role z aplikace.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-267">This starts fetching the roles from the application.</span></span>

       ![Konfigurovat jednotné přihlašování][41]

    > [!NOTE]
    > <span data-ttu-id="cf0b6-269">Spuštění služby Azure AD zřizování každých po určité době synchronizaci tyto role z AWS.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-269">Azure AD Provisioning service runs every after some time to sync the roles from AWS.</span></span> <span data-ttu-id="cf0b6-270">Měli byste vidět všechny zprostředkovatele Identity připojena AWS rolí do služby Azure AD a můžete je použít při přiřazování aplikace uživatelům nebo skupinám.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-270">You should see all the Identity Provider attached AWS roles into Azure AD and you can use them while assigning the application to users or groups.</span></span>

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cf0b6-271">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf0b6-271">Creating an Azure AD test user</span></span>
<span data-ttu-id="cf0b6-272">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-272">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cf0b6-274">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-274">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cf0b6-275">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-275">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cf0b6-277">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-277">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cf0b6-279">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-279">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cf0b6-281">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-281">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cf0b6-283">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-283">a.</span></span> <span data-ttu-id="cf0b6-284">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-284">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cf0b6-285">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-285">b.</span></span> <span data-ttu-id="cf0b6-286">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-286">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cf0b6-287">c.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-287">c.</span></span> <span data-ttu-id="cf0b6-288">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-288">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cf0b6-289">d.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-289">d.</span></span> <span data-ttu-id="cf0b6-290">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-290">Click **Create**.</span></span>
 
### <a name="creating-an-amazon-web-services-test-user"></a><span data-ttu-id="cf0b6-291">Vytváření testovacího uživatele Amazon Web Services</span><span class="sxs-lookup"><span data-stu-id="cf0b6-291">Creating an Amazon Web Services test user</span></span>

<span data-ttu-id="cf0b6-292">Pokud chcete povolit uživatelům Azure AD přihlášení k Amazon Web Services (AWS), se musí zřízená do Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-292">In order to enable Azure AD users to log in to Amazon Web Services (AWS), they must be provisioned into Amazon Web Services (AWS).</span></span> <span data-ttu-id="cf0b6-293">V případě Amazon Web Services (AWS) zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-293">In the case of Amazon Web Services (AWS), provisioning is a manual task.</span></span>

<span data-ttu-id="cf0b6-294">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-294">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="cf0b6-295">Přihlaste se k vaší **Amazon Web Services (AWS)** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-295">Log in to your **Amazon Web Services (AWS)** company site as administrator.</span></span>

2. <span data-ttu-id="cf0b6-296">Klikněte **konzoly domovské** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-296">Click the **Console Home** icon.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][11]

3. <span data-ttu-id="cf0b6-298">Klikněte na tlačítko identita a správa přístupu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-298">Click Identity and Access Management.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][28]

4. <span data-ttu-id="cf0b6-300">Na řídicím panelu klikněte na tlačítko **uživatelé**a potom klikněte na **vytvořte nové uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-300">In the Dashboard, click **Users**, and then click **Create New Users**.</span></span> 
   
    ![Konfigurovat jednotné přihlašování][29]

5. <span data-ttu-id="cf0b6-302">V dialogovém okně Vytvořit uživatele proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cf0b6-302">On the Create User dialog, perform the following steps:</span></span> 
   
    ![Konfigurovat jednotné přihlašování][30]   
    
    <span data-ttu-id="cf0b6-304">a.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-304">a.</span></span> <span data-ttu-id="cf0b6-305">V **zadejte uživatelská jména** textová pole, zadejte uživatelské jméno Brita Simon (userprincipalname) ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-305">In the **Enter User Names** textboxes, type Brita Simon's user name (userprincipalname) in Azure AD.</span></span>

    <span data-ttu-id="cf0b6-306">b.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-306">b.</span></span> <span data-ttu-id="cf0b6-307">Klikněte na tlačítko **vytvořit.**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-307">Click **Create.**</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cf0b6-308">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cf0b6-308">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cf0b6-309">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-309">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Amazon Web Services (AWS).</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cf0b6-311">**Pokud chcete přiřadit Britta Simon k Amazon Web Services (AWS), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cf0b6-311">**To assign Britta Simon to Amazon Web Services (AWS), perform the following steps:**</span></span>

1. <span data-ttu-id="cf0b6-312">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-312">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cf0b6-314">V seznamu aplikací vyberte **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-314">In the applications list, select **Amazon Web Services (AWS)**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. <span data-ttu-id="cf0b6-316">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-316">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cf0b6-318">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-318">Click **Add** button.</span></span> <span data-ttu-id="cf0b6-319">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-319">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cf0b6-321">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-321">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cf0b6-322">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-322">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cf0b6-323">Na **vybrat roli** , vyberte příslušnou roli pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-323">On **Select Role** tab, select the appropriate role for the user.</span></span> <span data-ttu-id="cf0b6-324">Tyto role jsou zobrazeny s názvem role a název zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-324">All these roles are shown with the role name and identity provider name.</span></span> <span data-ttu-id="cf0b6-325">Tímto způsobem můžete snadno identifikovat tyto role z AWS.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-325">This way you can easily identify the roles from AWS.</span></span>

8. <span data-ttu-id="cf0b6-326">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-326">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cf0b6-327">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cf0b6-327">Testing single sign-on</span></span>

<span data-ttu-id="cf0b6-328">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cf0b6-328">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cf0b6-329">Když kliknete na dlaždici Amazon Web Services (AWS) na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="cf0b6-329">When you click the Amazon Web Services (AWS) tile in the Access Panel, you should get automatically signed-on to your Amazon Web Services (AWS) application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cf0b6-330">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cf0b6-330">Additional resources</span></span>

* [<span data-ttu-id="cf0b6-331">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cf0b6-331">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cf0b6-332">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cf0b6-332">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
