---
title: "Kurz: Azure Active Directory integrace s Bambu podle Sprout sociálních | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Bambu podle Sprout sociálních."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2b9ddbc-cab7-40d6-aca1-5b171cab4199
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 985966d26f6ed0dcd4db47589abf94260ce62bf0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bambu-by-sprout-social"></a><span data-ttu-id="82142-103">Kurz: Azure Active Directory integrace s Bambu podle Sprout sociálních</span><span class="sxs-lookup"><span data-stu-id="82142-103">Tutorial: Azure Active Directory integration with Bambu by Sprout Social</span></span>

<span data-ttu-id="82142-104">V tomto kurzu zjistěte, jak integrovat Bambu podle Sprout sociálních s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="82142-104">In this tutorial, you learn how to integrate Bambu by Sprout Social with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="82142-105">Integrace Bambu podle Sprout sociálních s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="82142-105">Integrating Bambu by Sprout Social with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="82142-106">Můžete řídit ve službě Azure AD, který má přístup k Bambu podle Sprout sociálních</span><span class="sxs-lookup"><span data-stu-id="82142-106">You can control in Azure AD who has access to Bambu by Sprout Social</span></span>
- <span data-ttu-id="82142-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Bambu podle Sprout sociálních (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="82142-107">You can enable your users to automatically get signed-on to Bambu by Sprout Social (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="82142-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="82142-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="82142-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="82142-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Bambu by Sprout Social, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Bambu by Sprout Social.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="82142-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="82142-110">Prerequisites</span></span>

<span data-ttu-id="82142-111">Při konfiguraci integrace Azure AD s Bambu podle Sprout sociálních třeba následující položky:</span><span class="sxs-lookup"><span data-stu-id="82142-111">To configure Azure AD integration with Bambu by Sprout Social, you need the following items:</span></span>

- <span data-ttu-id="82142-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="82142-112">An Azure AD subscription</span></span>
- <span data-ttu-id="82142-113">Bambu podle Sprout sociálních jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="82142-113">A Bambu by Sprout Social single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82142-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="82142-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82142-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="82142-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82142-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="82142-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="82142-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82142-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="82142-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="82142-118">Scenario description</span></span>
<span data-ttu-id="82142-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="82142-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="82142-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="82142-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="82142-121">Přidání Bambu podle Sprout sociálních z Galerie</span><span class="sxs-lookup"><span data-stu-id="82142-121">Adding Bambu by Sprout Social from the gallery</span></span>
2. <span data-ttu-id="82142-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="82142-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bambu-by-sprout-social-from-the-gallery"></a><span data-ttu-id="82142-123">Přidání Bambu podle Sprout sociálních z Galerie</span><span class="sxs-lookup"><span data-stu-id="82142-123">Adding Bambu by Sprout Social from the gallery</span></span>
<span data-ttu-id="82142-124">Při konfiguraci integrace Bambu podle Sprout sociálních do služby Azure AD potřebujete přidat Bambu podle Sprout sociálních z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="82142-124">To configure the integration of Bambu by Sprout Social into Azure AD, you need to add Bambu by Sprout Social from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="82142-125">**Pokud chcete přidat Bambu podle Sprout sociálních z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="82142-125">**To add Bambu by Sprout Social from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="82142-126">V  **[portálu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="82142-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="82142-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="82142-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="82142-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="82142-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="82142-131">Klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno pro přidání nové aplikace.</span><span class="sxs-lookup"><span data-stu-id="82142-131">Click **New application** button on the top of the dialog to add new application.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="82142-133">Do vyhledávacího pole zadejte **Bambu podle Sprout sociálních**.</span><span class="sxs-lookup"><span data-stu-id="82142-133">In the search box, type **Bambu by Sprout Social**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_search.png)

5. <span data-ttu-id="82142-135">Na panelu výsledků vyberte **Bambu podle Sprout sociálních**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="82142-135">In the results panel, select **Bambu by Sprout Social**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="82142-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="82142-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="82142-138">V této části konfiguraci a testování Azure AD jednotné přihlašování s Bambu podle Sprout sociálních podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="82142-138">In this section, you configure and test Azure AD single sign-on with Bambu by Sprout Social based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="82142-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Bambu podle Sprout sociálních je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82142-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Bambu by Sprout Social is to a user in Azure AD.</span></span> <span data-ttu-id="82142-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Bambu podle Sprout sociálních musí navázat.</span><span class="sxs-lookup"><span data-stu-id="82142-140">In other words, a link relationship between an Azure AD user and the related user in Bambu by Sprout Social needs to be established.</span></span>

<span data-ttu-id="82142-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Bambu podle Sprout sociálních.</span><span class="sxs-lookup"><span data-stu-id="82142-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Bambu by Sprout Social.</span></span>

<span data-ttu-id="82142-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Bambu podle Sprout sociálních, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="82142-142">To configure and test Azure AD single sign-on with Bambu by Sprout Social, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="82142-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="82142-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="82142-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82142-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="82142-145">**[Vytváření Bambu uživatelem testovací Sprout sociálních](#creating-a-bambu-by-sprout-social-test-user)**  – Pokud chcete mít protějšek Britta Simon v Bambu podle Sprout sociálních propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="82142-145">**[Creating a Bambu by Sprout Social test user](#creating-a-bambu-by-sprout-social-test-user)** - to have a counterpart of Britta Simon in Bambu by Sprout Social that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="82142-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82142-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="82142-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="82142-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="82142-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82142-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="82142-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v vaše Bambu Sprout sociálních aplikací.</span><span class="sxs-lookup"><span data-stu-id="82142-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Bambu by Sprout Social application.</span></span>

<span data-ttu-id="82142-150">**Ke konfiguraci Azure AD jednotné přihlašování s Bambu podle Sprout sociálních, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="82142-150">**To configure Azure AD single sign-on with Bambu by Sprout Social, perform the following steps:**</span></span>

1. <span data-ttu-id="82142-151">Na portálu Azure na **Bambu podle Sprout sociálních** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="82142-151">In the Azure portal, on the **Bambu by Sprout Social** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="82142-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="82142-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_samlbase.png)

3. <span data-ttu-id="82142-155">Na **Bambu Sprout sociálních domény a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.</span><span class="sxs-lookup"><span data-stu-id="82142-155">On the **Bambu by Sprout Social Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_url.png)

4. <span data-ttu-id="82142-157">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="82142-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_certificate.png) 

5. <span data-ttu-id="82142-159">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82142-159">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="82142-161">Na **Bambu Sprout sociálních konfigurace** klikněte na tlačítko **konfigurace Bambu podle Sprout sociálních** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="82142-161">On the **Bambu by Sprout Social Configuration** section, click **Configure Bambu by Sprout Social** to open **Configure sign-on** window.</span></span> <span data-ttu-id="82142-162">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="82142-162">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_configure.png) 

7. <span data-ttu-id="82142-164">Konfigurace jednotného přihlašování na **Bambu podle Sprout sociálních** straně, budete muset odeslat stažené **soubor XML s metadaty** a **SAML jeden přihlašování adresa URL služby** k [Bambu podle Sprout sociálních podporu](mailto:support@getbambu.com).</span><span class="sxs-lookup"><span data-stu-id="82142-164">To configure single sign-on on **Bambu by Sprout Social** side, you need to send the downloaded **Metadata XML** and **SAML Single Sign-On Service URL** to [Bambu by Sprout Social support](mailto:support@getbambu.com).</span></span> <span data-ttu-id="82142-165">Toto se bude nastavení aby bylo možné používat jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="82142-165">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="82142-166">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="82142-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="82142-167">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** části, klikněte jednoduše na **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="82142-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click on the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="82142-168">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="82142-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

<!--### Next steps

To ensure users can sign-in to Bambu by Sprout Social after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Bambu by Sprout Social prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Bambu by Sprout Social in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Bambu by Sprout Social users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="82142-169">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="82142-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="82142-170">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82142-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="82142-172">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="82142-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="82142-173">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="82142-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="82142-175">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="82142-175">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="82142-177">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82142-177">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="82142-179">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="82142-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="82142-181">a.</span><span class="sxs-lookup"><span data-stu-id="82142-181">a.</span></span> <span data-ttu-id="82142-182">V **název** textovému poli, typ **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="82142-182">In the **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="82142-183">b.</span><span class="sxs-lookup"><span data-stu-id="82142-183">b.</span></span> <span data-ttu-id="82142-184">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82142-184">In the **User name** textbox, type the **email address** of Britta Simon.</span></span>

    <span data-ttu-id="82142-185">c.</span><span class="sxs-lookup"><span data-stu-id="82142-185">c.</span></span> <span data-ttu-id="82142-186">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="82142-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="82142-187">d.</span><span class="sxs-lookup"><span data-stu-id="82142-187">d.</span></span> <span data-ttu-id="82142-188">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="82142-188">Click **Create**.</span></span>
 
### <a name="creating-a-bambu-by-sprout-social-test-user"></a><span data-ttu-id="82142-189">Vytváření Bambu Sprout sociálních testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="82142-189">Creating a Bambu by Sprout Social test user</span></span>

<span data-ttu-id="82142-190">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele automaticky se vytvoří v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="82142-190">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="82142-191">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="82142-191">Assigning the Azure AD test user</span></span>

<span data-ttu-id="82142-192">V této části povolíte Britta Simon používat tak, že udělíte přístup k Bambu podle Sprout sociálních Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="82142-192">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Bambu by Sprout Social.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="82142-194">**Pokud chcete přiřadit Britta Simon Bambu podle Sprout sociálních, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="82142-194">**To assign Britta Simon to Bambu by Sprout Social, perform the following steps:**</span></span>

1. <span data-ttu-id="82142-195">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="82142-195">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="82142-197">V seznamu aplikací vyberte **Bambu podle Sprout sociálních**.</span><span class="sxs-lookup"><span data-stu-id="82142-197">In the applications list, select **Bambu by Sprout Social**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_app.png) 

3. <span data-ttu-id="82142-199">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="82142-199">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="82142-201">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="82142-201">Click **Add** button.</span></span> <span data-ttu-id="82142-202">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82142-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="82142-204">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="82142-204">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="82142-205">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82142-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="82142-206">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="82142-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="82142-207">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="82142-207">Testing single sign-on</span></span>

<span data-ttu-id="82142-208">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="82142-208">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="82142-209">Po kliknutí Bambu podle Sprout sociálních dlaždice na přístupovém panelu, jste měli získat automaticky přihlášení k vaší Bambu Sprout sociálních aplikací.</span><span class="sxs-lookup"><span data-stu-id="82142-209">When you click the Bambu by Sprout Social tile in the Access Panel, you should get automatically signed-on to your Bambu by Sprout Social application.</span></span> <span data-ttu-id="82142-210">Další podrobnosti o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="82142-210">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="82142-211">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="82142-211">Additional resources</span></span>

* [<span data-ttu-id="82142-212">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="82142-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82142-213">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="82142-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_203.png

