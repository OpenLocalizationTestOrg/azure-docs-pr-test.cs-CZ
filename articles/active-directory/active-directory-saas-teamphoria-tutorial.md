---
title: 'Kurz: Azure Active Directory integrace s Teamphoria | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Teamphoria."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a><span data-ttu-id="cebee-103">Kurz: Azure Active Directory integrace s Teamphoria</span><span class="sxs-lookup"><span data-stu-id="cebee-103">Tutorial: Azure Active Directory integration with Teamphoria</span></span>

<span data-ttu-id="cebee-104">V tomto kurzu zjistěte, jak integrovat Teamphoria s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cebee-104">In this tutorial, you learn how to integrate Teamphoria with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cebee-105">Integrace Teamphoria s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cebee-105">Integrating Teamphoria with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cebee-106">Můžete řídit ve službě Azure AD, který má přístup k Teamphoria</span><span class="sxs-lookup"><span data-stu-id="cebee-106">You can control in Azure AD who has access to Teamphoria</span></span>
- <span data-ttu-id="cebee-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Teamphoria (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cebee-107">You can enable your users to automatically get signed-on to Teamphoria (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cebee-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="cebee-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="cebee-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cebee-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="cebee-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cebee-110">Prerequisites</span></span>

<span data-ttu-id="cebee-111">Konfigurace integrace Azure AD s Teamphoria, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="cebee-111">To configure Azure AD integration with Teamphoria, you need the following items:</span></span>

- <span data-ttu-id="cebee-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cebee-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cebee-113">Teamphoria jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cebee-113">A Teamphoria single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cebee-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cebee-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cebee-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cebee-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cebee-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="cebee-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cebee-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cebee-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cebee-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cebee-118">Scenario description</span></span>
<span data-ttu-id="cebee-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cebee-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cebee-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cebee-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cebee-121">Přidání Teamphoria z Galerie</span><span class="sxs-lookup"><span data-stu-id="cebee-121">Adding Teamphoria from the gallery</span></span>
2. <span data-ttu-id="cebee-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cebee-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamphoria-from-the-gallery"></a><span data-ttu-id="cebee-123">Přidání Teamphoria z Galerie</span><span class="sxs-lookup"><span data-stu-id="cebee-123">Adding Teamphoria from the gallery</span></span>
<span data-ttu-id="cebee-124">Při konfiguraci integrace Teamphoria do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Teamphoria z galerie.</span><span class="sxs-lookup"><span data-stu-id="cebee-124">To configure the integration of Teamphoria into Azure AD, you need to add Teamphoria from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cebee-125">**Pokud chcete přidat Teamphoria z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cebee-125">**To add Teamphoria from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cebee-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cebee-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cebee-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cebee-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cebee-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cebee-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cebee-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cebee-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cebee-133">Do vyhledávacího pole zadejte **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="cebee-133">In the search box, type **Teamphoria**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. <span data-ttu-id="cebee-135">Na panelu výsledků vyberte **Teamphoria**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cebee-135">In the results panel, select **Teamphoria**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cebee-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cebee-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cebee-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Teamphoria podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="cebee-138">In this section, you configure and test Azure AD single sign-on with Teamphoria based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cebee-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Teamphoria je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cebee-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Teamphoria is to a user in Azure AD.</span></span> <span data-ttu-id="cebee-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Teamphoria musí navázat.</span><span class="sxs-lookup"><span data-stu-id="cebee-140">In other words, a link relationship between an Azure AD user and the related user in Teamphoria needs to be established.</span></span>

<span data-ttu-id="cebee-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="cebee-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Teamphoria.</span></span>

<span data-ttu-id="cebee-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Teamphoria, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="cebee-142">To configure and test Azure AD single sign-on with Teamphoria, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cebee-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="cebee-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cebee-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cebee-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cebee-145">**[Vytvoření zkušebního uživatele Teamphoria](#creating-a-teamphoria-test-user)**  – Pokud chcete mít protějšek Britta Simon v Teamphoria propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="cebee-145">**[Creating a Teamphoria test user](#creating-a-teamphoria-test-user)** - to have a counterpart of Britta Simon in Teamphoria that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="cebee-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cebee-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cebee-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cebee-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cebee-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cebee-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="cebee-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Teamphoria application.</span></span>

<span data-ttu-id="cebee-150">**Ke konfiguraci Azure AD jednotné přihlašování s Teamphoria, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cebee-150">**To configure Azure AD single sign-on with Teamphoria, perform the following steps:**</span></span>

1. <span data-ttu-id="cebee-151">Na portálu Azure Management portal na **Teamphoria** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cebee-151">In the Azure Management portal, on the **Teamphoria** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cebee-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="cebee-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. <span data-ttu-id="cebee-155">Na **Teamphoria domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cebee-155">On the **Teamphoria Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    <span data-ttu-id="cebee-157">a.</span><span class="sxs-lookup"><span data-stu-id="cebee-157">a.</span></span> <span data-ttu-id="cebee-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL, pomocí následujícího vzorce:`https://<sub-domain>.teamphoria.com/login`</span><span class="sxs-lookup"><span data-stu-id="cebee-158">In the **Sign-on URL** textbox, type the URL using the following pattern: `https://<sub-domain>.teamphoria.com/login`</span></span>    

    > [!NOTE] 
    > <span data-ttu-id="cebee-159">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cebee-159">Please note that these are not the real values.</span></span> <span data-ttu-id="cebee-160">Budete muset aktualizovat tyto hodnoty s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-160">You have to update these values with the actual Sign-On URL.</span></span> <span data-ttu-id="cebee-161">Obraťte se na [tým podpory Teamphoria klienta](https://www.teamphoria.com/) získat adresu URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-161">Contact [Teamphoria Client support team](https://www.teamphoria.com/) to get the Sign-on URL.</span></span> 

4. <span data-ttu-id="cebee-162">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte certifikát v počítači.</span><span class="sxs-lookup"><span data-stu-id="cebee-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. <span data-ttu-id="cebee-164">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cebee-164">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cebee-166">Na **Teamphoria konfigurace** klikněte na tlačítko **konfigurace Teamphoria** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cebee-166">On the **Teamphoria Configuration** section, click **Configure Teamphoria** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cebee-167">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cebee-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. <span data-ttu-id="cebee-169">Konfigurace jednotného přihlašování na **Teamphoria** straně, přihlášení do aplikace Teamphoria jako správce.</span><span class="sxs-lookup"><span data-stu-id="cebee-169">To configure single sign-on on **Teamphoria** side, Login to your Teamphoria application as an administrator.</span></span>

8. <span data-ttu-id="cebee-170">Přejděte na **nastavení správce** možnost v levém panelu nástrojů a v části kartě Konfigurace klikněte na **jeden přihlašování** a otevřete okno Konfigurace jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-170">Go to **ADMIN SETTINGS** option in the left toolbar and under the the Configure Tab click on **SINGLE SIGN-ON** to open the SSO configuration window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. <span data-ttu-id="cebee-172">Klikněte na **přidat nového poskytovatele IDENTITY** možnost v pravém horním rohu otevřete formulář pro přidání nastavení pro jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-172">Click on **ADD NEW IDENTITY PROVIDER** option in the top right corner to open the form for adding the settings for SSO.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. <span data-ttu-id="cebee-174">Zadejte podrobnosti v polích, jak je popsáno níže-</span><span class="sxs-lookup"><span data-stu-id="cebee-174">Enter the details in the fields as described below-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    <span data-ttu-id="cebee-176">a.</span><span class="sxs-lookup"><span data-stu-id="cebee-176">a.</span></span> <span data-ttu-id="cebee-177">**Zobrazit název** : Zadejte zobrazovaný název modulu plug-in na stránky pro správu.</span><span class="sxs-lookup"><span data-stu-id="cebee-177">**DISPLAY NAME** : Enter the display name of the plugin on the admin page.</span></span>

    <span data-ttu-id="cebee-178">b.</span><span class="sxs-lookup"><span data-stu-id="cebee-178">b.</span></span> <span data-ttu-id="cebee-179">**Název TLAČÍTKA** : název kartu, která se zobrazí na přihlašovací stránce pro přihlašování pomocí jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-179">**BUTTON NAME** : The name of the tab which will display on the login page for logging in via SSO.</span></span>

    <span data-ttu-id="cebee-180">c.</span><span class="sxs-lookup"><span data-stu-id="cebee-180">c.</span></span> <span data-ttu-id="cebee-181">**CERTIFIKÁT** : certifikát předtím stáhli z portálu Azure v poznámkovém bloku otevřete zkopírujte obsah stejné a vložte jej zde v poli.</span><span class="sxs-lookup"><span data-stu-id="cebee-181">**CERTIFICATE** : Open the Certificate downloaded earlier from the Azure portal in notepad, copy the contents of the same and paste it here in the box.</span></span>

    <span data-ttu-id="cebee-182">d.</span><span class="sxs-lookup"><span data-stu-id="cebee-182">d.</span></span> <span data-ttu-id="cebee-183">**VSTUPNÍ bod** : vložení **SAML jeden přihlašování adresa URL služby** zkopírovat starší formulář portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="cebee-183">**ENTRY POINT** : Paste the **SAML Single Sign-On Service URL** copied earlier form the Azure portal.</span></span>

    <span data-ttu-id="cebee-184">e.</span><span class="sxs-lookup"><span data-stu-id="cebee-184">e.</span></span> <span data-ttu-id="cebee-185">Přepněte možnost **ON** a klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cebee-185">Switch the option to **ON** and click on **SAVE**.</span></span>   

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cebee-186">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cebee-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="cebee-187">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cebee-187">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cebee-189">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cebee-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cebee-190">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cebee-190">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cebee-192">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cebee-192">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cebee-194">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cebee-194">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cebee-196">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cebee-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cebee-198">a.</span><span class="sxs-lookup"><span data-stu-id="cebee-198">a.</span></span> <span data-ttu-id="cebee-199">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cebee-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cebee-200">b.</span><span class="sxs-lookup"><span data-stu-id="cebee-200">b.</span></span> <span data-ttu-id="cebee-201">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cebee-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cebee-202">c.</span><span class="sxs-lookup"><span data-stu-id="cebee-202">c.</span></span> <span data-ttu-id="cebee-203">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cebee-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cebee-204">d.</span><span class="sxs-lookup"><span data-stu-id="cebee-204">d.</span></span> <span data-ttu-id="cebee-205">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cebee-205">Click **Create**.</span></span>
 
### <a name="creating-a-teamphoria-test-user"></a><span data-ttu-id="cebee-206">Vytvoření zkušebního uživatele Teamphoria</span><span class="sxs-lookup"><span data-stu-id="cebee-206">Creating a Teamphoria test user</span></span>

<span data-ttu-id="cebee-207">Pokud chcete povolit uživatelům Azure AD přihlášení do Teamphoria, musí být zřízená do Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="cebee-207">In order to enable Azure AD users to log into Teamphoria, they must be provisioned into Teamphoria.</span></span> <span data-ttu-id="cebee-208">V případě Teamphoria zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="cebee-208">In the case of Teamphoria, provisioning is a manual task.</span></span>

<span data-ttu-id="cebee-209">**Ke zřízení uživatelských účtů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cebee-209">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="cebee-210">Přihlaste se k serveru vaší společnosti Teamphoria jako správce.</span><span class="sxs-lookup"><span data-stu-id="cebee-210">Log in to your Teamphoria company site as an administrator.</span></span>

2. <span data-ttu-id="cebee-211">Klikněte na **správce** nastavení na levém panelu nástrojů a v části **SPRAVOVAT** kartě kliknutím na **uživatelé** chcete otevřít stránku Správce pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="cebee-211">Click on **ADMIN** settings on the left toolbar and under the **MANAGE** tab Click on **USERS** to open the admin page for users.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. <span data-ttu-id="cebee-213">Klikněte na **RUČNÍ POZVAT** možnost.</span><span class="sxs-lookup"><span data-stu-id="cebee-213">Click on the **MANUAL INVITE** option.</span></span>

    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. <span data-ttu-id="cebee-215">Na této stránce proveďte následující akce.</span><span class="sxs-lookup"><span data-stu-id="cebee-215">On this page, perform following action.</span></span> 
    
    ![Pozvat uživatele](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    <span data-ttu-id="cebee-217">a.</span><span class="sxs-lookup"><span data-stu-id="cebee-217">a.</span></span> <span data-ttu-id="cebee-218">V **e-MAILOVOU adresu** textovému poli, **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cebee-218">In the **EMAIL ADDRESS** textbox, the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cebee-219">b.</span><span class="sxs-lookup"><span data-stu-id="cebee-219">b.</span></span> <span data-ttu-id="cebee-220">V **KŘESTNÍ jméno** textovému poli, typ **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cebee-220">In the **FIRST NAME** textbox, type **Britta**.</span></span>

    <span data-ttu-id="cebee-221">c.</span><span class="sxs-lookup"><span data-stu-id="cebee-221">c.</span></span> <span data-ttu-id="cebee-222">V **příjmení** textovému poli, typ **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cebee-222">In the **LAST NAME** textbox, type **Simon**.</span></span>

    <span data-ttu-id="cebee-223">d.</span><span class="sxs-lookup"><span data-stu-id="cebee-223">d.</span></span> <span data-ttu-id="cebee-224">Klikněte na tlačítko **pozvání 1 uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cebee-224">Click **INVITE 1 USER**.</span></span> <span data-ttu-id="cebee-225">Uživatel musí přijmout pozvání k získání vytvořené v systému.</span><span class="sxs-lookup"><span data-stu-id="cebee-225">User needs to accept the invite to get created in the system.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cebee-226">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cebee-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cebee-227">V této části povolíte Britta Simon používat tak, že udělíte přístup k Teamphoria Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cebee-227">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Teamphoria.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cebee-229">**Pokud chcete přiřadit Britta Simon Teamphoria, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cebee-229">**To assign Britta Simon to Teamphoria, perform the following steps:**</span></span>

1. <span data-ttu-id="cebee-230">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cebee-230">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cebee-232">V seznamu aplikací vyberte **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="cebee-232">In the applications list, select **Teamphoria**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. <span data-ttu-id="cebee-234">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cebee-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cebee-236">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cebee-236">Click **Add** button.</span></span> <span data-ttu-id="cebee-237">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cebee-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cebee-239">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cebee-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cebee-240">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cebee-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cebee-241">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cebee-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cebee-242">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cebee-242">Testing single sign-on</span></span>

<span data-ttu-id="cebee-243">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cebee-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cebee-244">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="cebee-244">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="cebee-245">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="cebee-245">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cebee-246">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cebee-246">Additional resources</span></span>

* [<span data-ttu-id="cebee-247">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cebee-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cebee-248">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cebee-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

