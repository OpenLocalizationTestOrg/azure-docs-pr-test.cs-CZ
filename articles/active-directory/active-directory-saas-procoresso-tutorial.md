---
title: "Kurz: Azure Active Directory integration Procore jednotného přihlašování k | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Procore jednotné přihlašování."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 042a41eaa6bb21670b4c12068f1b017222f64b99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a><span data-ttu-id="e6437-103">Kurz: Azure Active Directory integrace s Procore jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-103">Tutorial: Azure Active Directory integration with Procore SSO</span></span>

<span data-ttu-id="e6437-104">V tomto kurzu zjistěte, jak integrovat Procore jednotné přihlašování s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e6437-104">In this tutorial, you learn how to integrate Procore SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e6437-105">Integrace Procore jednotné přihlašování s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e6437-105">Integrating Procore SSO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e6437-106">Můžete řídit ve službě Azure AD, který má přístup k Procore jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-106">You can control in Azure AD who has access to Procore SSO</span></span>
- <span data-ttu-id="e6437-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Procore jednotné přihlašování (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6437-107">You can enable your users to automatically get signed-on to Procore SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e6437-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="e6437-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="e6437-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e6437-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Procore SSO, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="e6437-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6437-110">Prerequisites</span></span>

<span data-ttu-id="e6437-111">Konfigurace integrace Azure AD s Procore jednotné přihlašování, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e6437-111">To configure Azure AD integration with Procore SSO, you need the following items:</span></span>

- <span data-ttu-id="e6437-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6437-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e6437-113">Procore SSO jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e6437-113">A Procore SSO single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e6437-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e6437-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e6437-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e6437-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e6437-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="e6437-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="e6437-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e6437-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e6437-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e6437-118">Scenario description</span></span>
<span data-ttu-id="e6437-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e6437-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e6437-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e6437-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e6437-121">Přidání Procore jednotného přihlašování z Galerie</span><span class="sxs-lookup"><span data-stu-id="e6437-121">Adding Procore SSO from the gallery</span></span>
2. <span data-ttu-id="e6437-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-procore-sso-from-the-gallery"></a><span data-ttu-id="e6437-123">Přidání Procore jednotného přihlašování z Galerie</span><span class="sxs-lookup"><span data-stu-id="e6437-123">Adding Procore SSO from the gallery</span></span>
<span data-ttu-id="e6437-124">Při konfiguraci integrace Procore přihlašování do služby Azure AD, potřebujete přidat Procore jednotného přihlašování z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e6437-124">To configure the integration of Procore SSO into Azure AD, you need to add Procore SSO from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e6437-125">**Pokud chcete přidat Procore jednotného přihlašování z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e6437-125">**To add Procore SSO from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e6437-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e6437-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e6437-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e6437-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e6437-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e6437-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e6437-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e6437-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e6437-133">Do vyhledávacího pole zadejte **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="e6437-133">In the search box, type **Procore SSO**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. <span data-ttu-id="e6437-135">Na panelu výsledků vyberte **Procore SSO**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6437-135">In the results panel, select **Procore SSO**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e6437-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e6437-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Procore jednotného přihlašování na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="e6437-138">In this section, you configure and test Azure AD single sign-on with Procore SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e6437-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku Procore SSO je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6437-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Procore SSO is to a user in Azure AD.</span></span> <span data-ttu-id="e6437-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Procore SSO musí navázat.</span><span class="sxs-lookup"><span data-stu-id="e6437-140">In other words, a link relationship between an Azure AD user and the related user in Procore SSO needs to be established.</span></span>

<span data-ttu-id="e6437-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** Procore sso.</span><span class="sxs-lookup"><span data-stu-id="e6437-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Procore SSO.</span></span>

<span data-ttu-id="e6437-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování služby Procore, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e6437-142">To configure and test Azure AD single sign-on with Procore SSO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e6437-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e6437-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e6437-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e6437-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e6437-145">**[Vytvoření zkušebního uživatele Procore SSO](#creating-a-procore-sso-test-user)**  – Pokud chcete mít protějšek Britta Simon Procore SSO propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="e6437-145">**[Creating a Procore SSO test user](#creating-a-procore-sso-test-user)** - to have a counterpart of Britta Simon in Procore SSO that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="e6437-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e6437-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e6437-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e6437-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e6437-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e6437-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci Procore jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e6437-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Procore SSO application.</span></span>

<span data-ttu-id="e6437-150">**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování služby Procore, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e6437-150">**To configure Azure AD single sign-on with Procore SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="e6437-151">Na portálu Azure Management portal na **Procore jednotného přihlašování k** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e6437-151">In the Azure Management portal, on the **Procore SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e6437-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="e6437-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. <span data-ttu-id="e6437-155">Na **Procore jednotného přihlašování k doméně a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="e6437-155">On the **Procore SSO Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. <span data-ttu-id="e6437-157">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="e6437-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. <span data-ttu-id="e6437-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6437-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e6437-161">Na **Procore Konfigurace jednotného přihlašování k** klikněte na tlačítko **Konfigurace jednotného přihlašování k Procore** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e6437-161">On the **Procore SSO Configuration** section, click **Configure Procore SSO** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e6437-162">Kopírování **SAML Entity ID a SAML jeden přihlašování adresu URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e6437-162">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. <span data-ttu-id="e6437-164">Konfigurace jednotného přihlašování na **Procore SSO** straně, přihlášení k serveru vaší společnosti procore jako správce.</span><span class="sxs-lookup"><span data-stu-id="e6437-164">To configure single sign-on on **Procore SSO** side, login to your procore company site as an administrator.</span></span>

8. <span data-ttu-id="e6437-165">V rozevíracím sady nástrojů dolů, klikněte na **správce** otevřete stránku nastavení jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e6437-165">From the toolbox drop down, click on **Admin** to open the SSO settings page.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. <span data-ttu-id="e6437-167">Vložení hodnoty do polí, jak je popsáno níže-</span><span class="sxs-lookup"><span data-stu-id="e6437-167">Paste the values in the boxes as described below-</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    <span data-ttu-id="e6437-169">a.</span><span class="sxs-lookup"><span data-stu-id="e6437-169">a.</span></span> <span data-ttu-id="e6437-170">V **jedné URL přihlašování na vystavitele** pole, vložte SAML Entity ID zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e6437-170">In the **Single Sign On Issuer URL** box, paste the SAML Entity ID copied from the Azure portal.</span></span>

    <span data-ttu-id="e6437-171">b.</span><span class="sxs-lookup"><span data-stu-id="e6437-171">b.</span></span> <span data-ttu-id="e6437-172">V **SAML přihlašovací na cílová adresa URL** pole, vložte SAML jeden přihlašování adresa URL služby zkopírovaných z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e6437-172">In the **SAML Sign On Target URL** box, paste the SAML Single Sign-On Service URL copied from the Azure portal.</span></span>

    <span data-ttu-id="e6437-173">c.</span><span class="sxs-lookup"><span data-stu-id="e6437-173">c.</span></span> <span data-ttu-id="e6437-174">Nyní otevřete **soubor XML s metadaty** výše si stáhli z portálu Azure a kopírovat ve značce s názvem certifikátu **certifikátu x 509**.</span><span class="sxs-lookup"><span data-stu-id="e6437-174">Now open the **Metadata XML** downloaded above from the Azure portal and copy the certficate in the tag named **X509Certificate**.</span></span> <span data-ttu-id="e6437-175">Vložit zkopírovaný hodnotu do **jednotné přihlašování x509 certifikát** pole.</span><span class="sxs-lookup"><span data-stu-id="e6437-175">Paste the copied value into the **Single Sign On x509 Certificate** box.</span></span>

10. <span data-ttu-id="e6437-176">Klikněte na **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="e6437-176">Click on **Save Changes**.</span></span>

11. <span data-ttu-id="e6437-177">Po tato nastavení, musí poslat **název domény** (např **contoso.com**) prostřednictvím které jsou do Procore k protokolování [tým podpory Procore](https://support.procore.com/) a aktivují federované jednotné přihlašování pro tuto doménu.</span><span class="sxs-lookup"><span data-stu-id="e6437-177">After these settings, you needs to send the **domain name** (e.g **contoso.com**) through which you are logging into Procore to the [Procore Support team](https://support.procore.com/) and they will activate federated SSO for that domain.</span></span>

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e6437-178">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6437-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="e6437-179">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e6437-179">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e6437-181">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e6437-181">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e6437-182">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e6437-182">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e6437-184">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e6437-184">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e6437-186">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e6437-186">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e6437-188">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e6437-188">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e6437-190">a.</span><span class="sxs-lookup"><span data-stu-id="e6437-190">a.</span></span> <span data-ttu-id="e6437-191">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e6437-191">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e6437-192">b.</span><span class="sxs-lookup"><span data-stu-id="e6437-192">b.</span></span> <span data-ttu-id="e6437-193">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e6437-193">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e6437-194">c.</span><span class="sxs-lookup"><span data-stu-id="e6437-194">c.</span></span> <span data-ttu-id="e6437-195">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e6437-195">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e6437-196">d.</span><span class="sxs-lookup"><span data-stu-id="e6437-196">d.</span></span> <span data-ttu-id="e6437-197">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e6437-197">Click **Create**.</span></span>
 
### <a name="creating-a-procore-sso-test-user"></a><span data-ttu-id="e6437-198">Vytvoření zkušebního uživatele Procore jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-198">Creating a Procore SSO test user</span></span>

<span data-ttu-id="e6437-199">Postupujte níže uvedených pokynů pro vytvoření Procore testovací uživatele na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="e6437-199">Please follow the below steps to create a Procore test user on their side.</span></span>

1. <span data-ttu-id="e6437-200">Přihlášení k serveru vaší společnosti procore jako správce.</span><span class="sxs-lookup"><span data-stu-id="e6437-200">Login to your procore company site as an administrator.</span></span>  

2. <span data-ttu-id="e6437-201">V rozevíracím sady nástrojů dolů, klikněte na **Directory** chcete otevřít stránku directory společnosti.</span><span class="sxs-lookup"><span data-stu-id="e6437-201">From the toolbox drop down, click on **Directory** to open the company directory page.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. <span data-ttu-id="e6437-203">Klikněte na **přidat osobu** možnost Otevřít formulář a zadejte provést následující možnosti -</span><span class="sxs-lookup"><span data-stu-id="e6437-203">Click on **Add a Person** option to open the form and enter perform following options -</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    <span data-ttu-id="e6437-205">a.</span><span class="sxs-lookup"><span data-stu-id="e6437-205">a.</span></span> <span data-ttu-id="e6437-206">V **křestní jméno** textovému poli, křestní jméno uživatele typu jako **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e6437-206">In the **First Name** textbox, type user's first name like **Britta**.</span></span>

    <span data-ttu-id="e6437-207">b.</span><span class="sxs-lookup"><span data-stu-id="e6437-207">b.</span></span> <span data-ttu-id="e6437-208">V **příjmení** textovému poli, příjmení uživatele typu jako **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e6437-208">In the **Last name** textbox, type user's last name like **Simon**.</span></span>

    <span data-ttu-id="e6437-209">c.</span><span class="sxs-lookup"><span data-stu-id="e6437-209">c.</span></span> <span data-ttu-id="e6437-210">V **e-mailovou adresu** textovému poli, typu uživatele e-mailovou adresu jako  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="e6437-210">In the **Email Address** textbox, type user's email address like **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="e6437-211">d.</span><span class="sxs-lookup"><span data-stu-id="e6437-211">d.</span></span> <span data-ttu-id="e6437-212">Vyberte **šablona oprávnění** jako **později použít šablonu oprávnění**.</span><span class="sxs-lookup"><span data-stu-id="e6437-212">Select **Permission Template** as **Apply Permission Template Later**.</span></span>

    <span data-ttu-id="e6437-213">e.</span><span class="sxs-lookup"><span data-stu-id="e6437-213">e.</span></span> <span data-ttu-id="e6437-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e6437-214">Click **Create**.</span></span>

4. <span data-ttu-id="e6437-215">Zkontrolovat a aktualizovat podrobnosti nově přidané kontakt.</span><span class="sxs-lookup"><span data-stu-id="e6437-215">Check and update the details for the newly added contact.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. <span data-ttu-id="e6437-217">Klikněte na **uložit a odeslat Invitiation** (Pokud pozvání prostřednictvím e-mailu je vyžadována) nebo **Uložit** (uložit přímo) k dokončení registrace uživatele.</span><span class="sxs-lookup"><span data-stu-id="e6437-217">Click on **Save and Send Invitiation** (if an invite through mail is required) or **Save** (Save directly) to complete the user registration.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e6437-219">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6437-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e6437-220">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k Procore jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e6437-220">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Procore SSO.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e6437-222">**Pokud chcete přiřadit Britta Simon Procore jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e6437-222">**To assign Britta Simon to Procore SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="e6437-223">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e6437-223">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e6437-225">V seznamu aplikací vyberte **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="e6437-225">In the applications list, select **Procore SSO**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. <span data-ttu-id="e6437-227">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e6437-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e6437-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6437-229">Click **Add** button.</span></span> <span data-ttu-id="e6437-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e6437-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e6437-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e6437-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e6437-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e6437-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e6437-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e6437-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e6437-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e6437-235">Testing single sign-on</span></span>

<span data-ttu-id="e6437-236">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="e6437-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e6437-237">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="e6437-237">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="e6437-238">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="e6437-238">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> <span data-ttu-id="e6437-239">Když kliknete na dlaždici Procore jednotného přihlašování na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Procore jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e6437-239">When you click the Procore SSO tile in the Access Panel, you should get automatically signed-on to your Procore SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6437-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e6437-240">Additional resources</span></span>

* [<span data-ttu-id="e6437-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e6437-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e6437-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e6437-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

