---
title: "Kurz: Azure Active Directory integrace s Mimecast osobní portál | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Mimecast osobní portál."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: bf46da35a55608d7e4656c9dd3ad9d5f2253e225
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a><span data-ttu-id="2c7c2-103">Kurz: Azure Active Directory integrace s Mimecast osobní portálu</span><span class="sxs-lookup"><span data-stu-id="2c7c2-103">Tutorial: Azure Active Directory integration with Mimecast Personal Portal</span></span>

<span data-ttu-id="2c7c2-104">V tomto kurzu zjistěte, jak integrovat Mimecast osobní portálu v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2c7c2-104">In this tutorial, you learn how to integrate Mimecast Personal Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2c7c2-105">Integrace portálu osobní Mimecast s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-105">Integrating Mimecast Personal Portal with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2c7c2-106">Můžete řídit ve službě Azure AD, který má přístup k portálu osobní Mimecast</span><span class="sxs-lookup"><span data-stu-id="2c7c2-106">You can control in Azure AD who has access to Mimecast Personal Portal</span></span>
- <span data-ttu-id="2c7c2-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k portálu osobní Mimecast (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c7c2-107">You can enable your users to automatically get signed-on to Mimecast Personal Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c7c2-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2c7c2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2c7c2-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2c7c2-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2c7c2-110">Prerequisites</span></span>

<span data-ttu-id="2c7c2-111">Ke konfiguraci integrace služby Azure AD pomocí portálu osobní Mimecast, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-111">To configure Azure AD integration with Mimecast Personal Portal, you need the following items:</span></span>

- <span data-ttu-id="2c7c2-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c7c2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2c7c2-113">Portálu osobní Mimecast jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2c7c2-113">A Mimecast Personal Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2c7c2-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2c7c2-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c7c2-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2c7c2-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c7c2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2c7c2-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2c7c2-118">Scenario description</span></span>
<span data-ttu-id="2c7c2-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2c7c2-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c7c2-121">Přidání osobní portálu Mimecast z Galerie</span><span class="sxs-lookup"><span data-stu-id="2c7c2-121">Adding Mimecast Personal Portal from the gallery</span></span>
2. <span data-ttu-id="2c7c2-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c7c2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-personal-portal-from-the-gallery"></a><span data-ttu-id="2c7c2-123">Přidání osobní portálu Mimecast z Galerie</span><span class="sxs-lookup"><span data-stu-id="2c7c2-123">Adding Mimecast Personal Portal from the gallery</span></span>
<span data-ttu-id="2c7c2-124">Při konfiguraci integrace Mimecast osobní portálu do služby Azure AD, potřebujete přidat osobní portál Mimecast z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-124">To configure the integration of Mimecast Personal Portal into Azure AD, you need to add Mimecast Personal Portal from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2c7c2-125">**Pokud chcete přidat osobní portál Mimecast z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2c7c2-125">**To add Mimecast Personal Portal from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2c7c2-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2c7c2-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2c7c2-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2c7c2-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2c7c2-133">Do vyhledávacího pole zadejte **Mimecast osobní portál**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-133">In the search box, type **Mimecast Personal Portal**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. <span data-ttu-id="2c7c2-135">Na panelu výsledků vyberte **osobní portál Mimecast**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-135">In the results panel, select **Mimecast Personal Portal**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2c7c2-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c7c2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2c7c2-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mimecast osobní portál podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2c7c2-138">In this section, you configure and test Azure AD single sign-on with Mimecast Personal Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2c7c2-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku osobní portálu Mimecast je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mimecast Personal Portal is to a user in Azure AD.</span></span> <span data-ttu-id="2c7c2-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Mimecast osobní portálu musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-140">In other words, a link relationship between an Azure AD user and the related user in Mimecast Personal Portal needs to be established.</span></span>

<span data-ttu-id="2c7c2-141">Osobní portálu Mimecast přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-141">In Mimecast Personal Portal, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2c7c2-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Mimecast osobní portál, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-142">To configure and test Azure AD single sign-on with Mimecast Personal Portal, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2c7c2-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2c7c2-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c7c2-145">**[Vytvoření zkušebního uživatele osobní portál Mimecast](#creating-a-mimecast-personal-portal-test-user)**  – Pokud chcete mít protějšek Britta Simon v Mimecast osobní portál, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-145">**[Creating a Mimecast Personal Portal test user](#creating-a-mimecast-personal-portal-test-user)** - to have a counterpart of Britta Simon in Mimecast Personal Portal that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2c7c2-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c7c2-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2c7c2-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c7c2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2c7c2-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Mimecast osobní portál.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mimecast Personal Portal application.</span></span>

<span data-ttu-id="2c7c2-150">**Ke konfiguraci Azure AD jednotné přihlašování s Mimecast osobní portál, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2c7c2-150">**To configure Azure AD single sign-on with Mimecast Personal Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="2c7c2-151">Na portálu Azure na **osobní portál Mimecast** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-151">In the Azure portal, on the **Mimecast Personal Portal** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2c7c2-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. <span data-ttu-id="2c7c2-155">Na **Mimecast osobní portál domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-155">On the **Mimecast Personal Portal Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    <span data-ttu-id="2c7c2-157">a.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-157">a.</span></span> <span data-ttu-id="2c7c2-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    <span data-ttu-id="2c7c2-159">b.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-159">b.</span></span> <span data-ttu-id="2c7c2-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > <span data-ttu-id="2c7c2-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-161">These values are not real.</span></span> <span data-ttu-id="2c7c2-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2c7c2-163">Obraťte se na [tým podpory Mimecast osobní portál klienta](https://www.mimecast.com/customer-success/technical-support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-163">Contact [Mimecast Personal Portal Client support team](https://www.mimecast.com/customer-success/technical-support/) to get these values.</span></span> 
 


4. <span data-ttu-id="2c7c2-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. <span data-ttu-id="2c7c2-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2c7c2-168">Na **osobní konfigurace portálu Mimecast** klikněte na tlačítko **nakonfigurovat portál osobní Mimecast** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-168">On the **Mimecast Personal Portal Configuration** section, click **Configure Mimecast Personal Portal** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2c7c2-169">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="2c7c2-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. <span data-ttu-id="2c7c2-171">V okně prohlížeče jiný web Přihlaste se k portálu osobní Mimecast jako správce.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-171">In a different web browser window, log into your Mimecast Personal Portal as an administrator.</span></span>

8. <span data-ttu-id="2c7c2-172">Přejděte na **služby \> aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-172">Go to **Services \> Applications**.</span></span>
   
    <span data-ttu-id="2c7c2-173">![Aplikace](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "aplikace")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-173">![Applications](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "Applications")</span></span>

9. <span data-ttu-id="2c7c2-174">Klikněte na tlačítko **ověřování profily**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-174">Click **Authentication Profiles**.</span></span>
   
    <span data-ttu-id="2c7c2-175">![Profily ověřování](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "profily ověřování")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-175">![Authentication Profiles](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")</span></span>

10. <span data-ttu-id="2c7c2-176">Klikněte na tlačítko **nový profil ověřování**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-176">Click **New Authentication Profile**.</span></span>
   
    <span data-ttu-id="2c7c2-177">![Nový profil ověřování](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "nový profil ověřování")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-177">![New Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile")</span></span>

11. <span data-ttu-id="2c7c2-178">V **profil ověření** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-178">In the **Authentication Profile** section, perform the following steps:</span></span>
   
    <span data-ttu-id="2c7c2-179">![Profil ověření](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "profil ověření")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-179">![Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")</span></span>
   
    <span data-ttu-id="2c7c2-180">a.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-180">a.</span></span> <span data-ttu-id="2c7c2-181">V **popis** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-181">In the **Description** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="2c7c2-182">b.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-182">b.</span></span> <span data-ttu-id="2c7c2-183">Vyberte **vynutit ověřování SAML pro portál Mimecast osobní**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-183">Select **Enforce SAML Authentication for Mimecast Personal Portal**.</span></span>
   
    <span data-ttu-id="2c7c2-184">c.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-184">c.</span></span> <span data-ttu-id="2c7c2-185">Jako **zprostředkovatele**, vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-185">As **Provider**, select **Azure Active Directory**.</span></span>
   
    <span data-ttu-id="2c7c2-186">d.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-186">d.</span></span> <span data-ttu-id="2c7c2-187">V **URL vystavitele** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-187">In **Issuer URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="2c7c2-188">e.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-188">e.</span></span> <span data-ttu-id="2c7c2-189">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-189">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="2c7c2-190">f.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-190">f.</span></span> <span data-ttu-id="2c7c2-191">V **adresy URL odhlašovací** textovému poli, vložte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-191">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2c7c2-192">g.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-192">g.</span></span> <span data-ttu-id="2c7c2-193">Otevřete váš **kódování base-64** kódovaného certifikátu v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **certifikát zprostředkovatele Identity (Metadata)** textové pole.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-193">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **Identity Provider Certificate (Metadata)** textbox.</span></span>

    <span data-ttu-id="2c7c2-194">h.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-194">h.</span></span> <span data-ttu-id="2c7c2-195">Vyberte **povolit jednotné přihlašování na**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-195">Select **Allow Single Sign On**.</span></span>
   
    <span data-ttu-id="2c7c2-196">i.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-196">i.</span></span> <span data-ttu-id="2c7c2-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-197">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="2c7c2-198">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2c7c2-198">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2c7c2-199">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-199">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2c7c2-200">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2c7c2-200">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2c7c2-201">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c7c2-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="2c7c2-202">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-202">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2c7c2-204">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2c7c2-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2c7c2-205">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-205">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2c7c2-207">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-207">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2c7c2-209">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-209">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c7c2-211">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2c7c2-213">a.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-213">a.</span></span> <span data-ttu-id="2c7c2-214">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-214">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2c7c2-215">b.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-215">b.</span></span> <span data-ttu-id="2c7c2-216">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-216">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2c7c2-217">c.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-217">c.</span></span> <span data-ttu-id="2c7c2-218">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-218">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2c7c2-219">d.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-219">d.</span></span> <span data-ttu-id="2c7c2-220">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-220">Click **Create**.</span></span>
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a><span data-ttu-id="2c7c2-221">Vytvoření zkušebního uživatele Mimecast osobní portálu</span><span class="sxs-lookup"><span data-stu-id="2c7c2-221">Creating a Mimecast Personal Portal test user</span></span>

<span data-ttu-id="2c7c2-222">Pokud chcete povolit uživatelům Azure AD přihlášení na portál osobní Mimecast, musí být zřízená Mimecast osobní portálu.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-222">In order to enable Azure AD users to log into Mimecast Personal Portal, they must be provisioned into Mimecast Personal Portal.</span></span> <span data-ttu-id="2c7c2-223">V případě Mimecast osobní portál je zřizování ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-223">In the case of Mimecast Personal Portal, provisioning is a manual task.</span></span>

<span data-ttu-id="2c7c2-224">Budete muset registraci domény, před vytvořením uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-224">You need to register a domain before you can create users.</span></span>

<span data-ttu-id="2c7c2-225">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2c7c2-225">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="2c7c2-226">Přihlaste se k vaší **Mimecast osobní portál** jako správce.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-226">Sign on to your **Mimecast Personal Portal** as administrator.</span></span>

2. <span data-ttu-id="2c7c2-227">Přejděte na **adresáře \> interní**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-227">Go to **Directories \> Internal**.</span></span>
   
    <span data-ttu-id="2c7c2-228">![Adresáře](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "adresáře")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-228">![Directories](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "Directories")</span></span>

3. <span data-ttu-id="2c7c2-229">Klikněte na tlačítko **zaregistrovat nové domény**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-229">Click **Register New Domain**.</span></span>
   
    <span data-ttu-id="2c7c2-230">![Zaregistrujte novou doménu](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "zaregistrovat nové domény")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-230">![Register New Domain](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")</span></span>

4. <span data-ttu-id="2c7c2-231">Po vytvoření nové domény, klikněte na tlačítko **novou adresu**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-231">After your new domain has been created, click **New Address**.</span></span>
   
    <span data-ttu-id="2c7c2-232">![Nové adresy](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "novou adresu")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-232">![New Address](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "New Address")</span></span>

5. <span data-ttu-id="2c7c2-233">V dialogovém okně nové adresy, proveďte následující kroky platný Azure chcete zřídit účet AD:</span><span class="sxs-lookup"><span data-stu-id="2c7c2-233">In the new address dialog, perform the following steps of a valid Azure AD account you want to provision:</span></span>
   
    <span data-ttu-id="2c7c2-234">![Uložit](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "uložit")</span><span class="sxs-lookup"><span data-stu-id="2c7c2-234">![Save](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Save")</span></span>
   
    <span data-ttu-id="2c7c2-235">a.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-235">a.</span></span> <span data-ttu-id="2c7c2-236">V **e-mailovou adresu** textovému poli, typ **e-mailovou adresu** uživatele jako  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2c7c2-236">In the **Email Address** textbox, type **Email Address** of the user as **BrittaSimon@contoso.com**.</span></span>
    
    <span data-ttu-id="2c7c2-237">b.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-237">b.</span></span> <span data-ttu-id="2c7c2-238">V **globální název** textovému poli, typ **uživatelské jméno** jako **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-238">In the **Global Name** textbox, type the **username** as **BrittaSimon**.</span></span>

    <span data-ttu-id="2c7c2-239">c.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-239">c.</span></span> <span data-ttu-id="2c7c2-240">V **heslo**, a **Potvrdit heslo** textová pole, zadejte **heslo** uživatele.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-240">In the **Password**, and **Confirm Password** textboxes, type the **Password** of the user.</span></span>
   
    <span data-ttu-id="2c7c2-241">b.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-241">b.</span></span> <span data-ttu-id="2c7c2-242">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-242">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="2c7c2-243">Další nástroje pro tvorbu účet uživatele Mimecast osobní portál nebo rozhraní API poskytovaných Mimecast osobní portálu můžete použít ke zřízení uživatelských účtů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-243">You can use any other Mimecast Personal Portal user account creation tools or APIs provided by Mimecast Personal Portal to provision Azure AD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2c7c2-244">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c7c2-244">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2c7c2-245">V této části povolíte Britta Simon používat tak, že udělíte přístup na portál osobní Mimecast Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mimecast Personal Portal.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2c7c2-247">**Pokud chcete přiřadit Britta Simon Mimecast osobní portál, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2c7c2-247">**To assign Britta Simon to Mimecast Personal Portal, perform the following steps:**</span></span>

1. <span data-ttu-id="2c7c2-248">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2c7c2-250">V seznamu aplikací vyberte **Mimecast osobní portál**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-250">In the applications list, select **Mimecast Personal Portal**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. <span data-ttu-id="2c7c2-252">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2c7c2-254">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-254">Click **Add** button.</span></span> <span data-ttu-id="2c7c2-255">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2c7c2-257">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2c7c2-258">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2c7c2-259">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2c7c2-260">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2c7c2-260">Testing single sign-on</span></span>
<span data-ttu-id="2c7c2-261">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2c7c2-262">Když kliknete na dlaždici Mimecast osobní portál na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Mimecast osobní portál.</span><span class="sxs-lookup"><span data-stu-id="2c7c2-262">When you click the Mimecast Personal Portal tile in the Access Panel, you should get automatically signed-on to your Mimecast Personal Portal application.</span></span> <span data-ttu-id="2c7c2-263">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2c7c2-263">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2c7c2-264">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2c7c2-264">Additional resources</span></span>

* [<span data-ttu-id="2c7c2-265">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2c7c2-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c7c2-266">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2c7c2-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

