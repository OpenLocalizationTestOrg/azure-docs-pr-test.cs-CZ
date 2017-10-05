---
title: 'Kurz: Azure Active Directory integrace s ThousandEyes | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ThousandEyes."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: 392add7d5f0a55598b8b90760f5c3f2d1e67ac02
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a><span data-ttu-id="10b2b-103">Kurz: Azure Active Directory integrace s ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="10b2b-103">Tutorial: Azure Active Directory integration with ThousandEyes</span></span>

<span data-ttu-id="10b2b-104">V tomto kurzu zjistěte, jak integrovat ThousandEyes s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="10b2b-104">In this tutorial, you learn how to integrate ThousandEyes with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="10b2b-105">Integrace ThousandEyes s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="10b2b-105">Integrating ThousandEyes with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="10b2b-106">Můžete řídit ve službě Azure AD, který má přístup k ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="10b2b-106">You can control in Azure AD who has access to ThousandEyes</span></span>
- <span data-ttu-id="10b2b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ThousandEyes (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="10b2b-107">You can enable your users to automatically get signed-on to ThousandEyes (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="10b2b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="10b2b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="10b2b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="10b2b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10b2b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="10b2b-110">Prerequisites</span></span>

<span data-ttu-id="10b2b-111">Konfigurace integrace Azure AD s ThousandEyes, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="10b2b-111">To configure Azure AD integration with ThousandEyes, you need the following items:</span></span>

- <span data-ttu-id="10b2b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="10b2b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="10b2b-113">ThousandEyes jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="10b2b-113">A ThousandEyes single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="10b2b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="10b2b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="10b2b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="10b2b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="10b2b-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="10b2b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="10b2b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="10b2b-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="10b2b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="10b2b-118">Scenario description</span></span>
<span data-ttu-id="10b2b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="10b2b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="10b2b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="10b2b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="10b2b-121">Přidání ThousandEyes z Galerie</span><span class="sxs-lookup"><span data-stu-id="10b2b-121">Adding ThousandEyes from the gallery</span></span>
2. <span data-ttu-id="10b2b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="10b2b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thousandeyes-from-the-gallery"></a><span data-ttu-id="10b2b-123">Přidání ThousandEyes z Galerie</span><span class="sxs-lookup"><span data-stu-id="10b2b-123">Adding ThousandEyes from the gallery</span></span>
<span data-ttu-id="10b2b-124">Při konfiguraci integrace ThousandEyes do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ThousandEyes z galerie.</span><span class="sxs-lookup"><span data-stu-id="10b2b-124">To configure the integration of ThousandEyes into Azure AD, you need to add ThousandEyes from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="10b2b-125">**Pokud chcete přidat ThousandEyes z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="10b2b-125">**To add ThousandEyes from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="10b2b-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="10b2b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="10b2b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="10b2b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="10b2b-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="10b2b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="10b2b-133">Do vyhledávacího pole zadejte **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-133">In the search box, type **ThousandEyes**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. <span data-ttu-id="10b2b-135">Na panelu výsledků vyberte **ThousandEyes**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="10b2b-135">In the results panel, select **ThousandEyes**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="10b2b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="10b2b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="10b2b-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ThousandEyes podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="10b2b-138">In this section, you configure and test Azure AD single sign-on with ThousandEyes based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="10b2b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ThousandEyes je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="10b2b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ThousandEyes is to a user in Azure AD.</span></span> <span data-ttu-id="10b2b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ThousandEyes musí navázat.</span><span class="sxs-lookup"><span data-stu-id="10b2b-140">In other words, a link relationship between an Azure AD user and the related user in ThousandEyes needs to be established.</span></span>

<span data-ttu-id="10b2b-141">V ThousandEyes, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="10b2b-141">In ThousandEyes, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="10b2b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ThousandEyes, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="10b2b-142">To configure and test Azure AD single sign-on with ThousandEyes, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="10b2b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="10b2b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="10b2b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="10b2b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="10b2b-145">**[Vytvoření zkušebního uživatele ThousandEyes](#creating-a-thousandeyes-test-user)**  – Pokud chcete mít protějšek Britta Simon v ThousandEyes propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="10b2b-145">**[Creating a ThousandEyes test user](#creating-a-thousandeyes-test-user)** - to have a counterpart of Britta Simon in ThousandEyes that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="10b2b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="10b2b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="10b2b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="10b2b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="10b2b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="10b2b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="10b2b-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="10b2b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ThousandEyes application.</span></span>

<span data-ttu-id="10b2b-150">**Ke konfiguraci Azure AD jednotné přihlašování s ThousandEyes, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="10b2b-150">**To configure Azure AD single sign-on with ThousandEyes, perform the following steps:**</span></span>

1. <span data-ttu-id="10b2b-151">Na portálu Azure na **ThousandEyes** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-151">In the Azure portal, on the **ThousandEyes** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="10b2b-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="10b2b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. <span data-ttu-id="10b2b-155">Na **ThousandEyes domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10b2b-155">On the **ThousandEyes Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    <span data-ttu-id="10b2b-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://app.thousandeyes.com/login/sso`</span><span class="sxs-lookup"><span data-stu-id="10b2b-157">In the **Sign-on URL** textbox, type a URL as: `https://app.thousandeyes.com/login/sso`</span></span>

4. <span data-ttu-id="10b2b-158">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="10b2b-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. <span data-ttu-id="10b2b-160">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="10b2b-160">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="10b2b-162">Na **ThousandEyes konfigurace** klikněte na tlačítko **konfigurace ThousandEyes** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="10b2b-162">On the **ThousandEyes Configuration** section, click **Configure ThousandEyes** to open **Configure sign-on** window.</span></span> <span data-ttu-id="10b2b-163">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="10b2b-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. <span data-ttu-id="10b2b-165">V okně prohlížeče jiný web, přihlaste se k vaší **ThousandEyes** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="10b2b-165">In a different web browser window, sign on to your **ThousandEyes** company site as an administrator.</span></span>

8. <span data-ttu-id="10b2b-166">V nabídce v horní části, klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-166">In the menu on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="10b2b-167">![Nastavení](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="10b2b-167">![Settings](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "Settings")</span></span>

9. <span data-ttu-id="10b2b-168">Klikněte na tlačítko **účtu**</span><span class="sxs-lookup"><span data-stu-id="10b2b-168">Click **Account**</span></span>
   
    <span data-ttu-id="10b2b-169">![Účet](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="10b2b-169">![Account](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Account")</span></span>

10. <span data-ttu-id="10b2b-170">Klikněte **ověřování a zabezpečení** kartě.</span><span class="sxs-lookup"><span data-stu-id="10b2b-170">Click the **Security & Authentication** tab.</span></span>
   
    <span data-ttu-id="10b2b-171">![Zabezpečení a ověřování](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "ověřování a zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="10b2b-171">![Security & Authentication](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "Security & Authentication")</span></span>

11. <span data-ttu-id="10b2b-172">V **instalace Single Sign-On** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10b2b-172">In the **Setup Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="10b2b-173">![Nastavení jednotného přihlašování](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "nastavit jednotné přihlašování")</span><span class="sxs-lookup"><span data-stu-id="10b2b-173">![Setup Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Setup Single Sign-On")</span></span>
  
    <span data-ttu-id="10b2b-174">a.</span><span class="sxs-lookup"><span data-stu-id="10b2b-174">a.</span></span> <span data-ttu-id="10b2b-175">Vyberte **povolit jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-175">Select **Enable Single Sign-On**.</span></span>
  
    <span data-ttu-id="10b2b-176">b.</span><span class="sxs-lookup"><span data-stu-id="10b2b-176">b.</span></span> <span data-ttu-id="10b2b-177">V **URL přihlašovací stránky** textovému poli, vložte **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="10b2b-177">In **Login Page URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="10b2b-178">c.</span><span class="sxs-lookup"><span data-stu-id="10b2b-178">c.</span></span> <span data-ttu-id="10b2b-179">V **adresa URL odhlašovací stránky** textovému poli, vložte **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="10b2b-179">In **Logout Page URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="10b2b-180">d.</span><span class="sxs-lookup"><span data-stu-id="10b2b-180">d.</span></span> <span data-ttu-id="10b2b-181">**Vystavitel zprostředkovatele identity** textovému poli, vložte **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="10b2b-181">**Identity Provider Issuer** textbox, paste **SAML Entity ID** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="10b2b-182">e.</span><span class="sxs-lookup"><span data-stu-id="10b2b-182">e.</span></span> <span data-ttu-id="10b2b-183">V **ověřovací certifikát**, klikněte na tlačítko **zvolte soubor**a pak nahrajte certifikát, který jste si stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="10b2b-183">In **Verification Certificate**, click **Choose file**, and then upload the certificate you have downloaded from Azure portal.</span></span>
  
    <span data-ttu-id="10b2b-184">f.</span><span class="sxs-lookup"><span data-stu-id="10b2b-184">f.</span></span> <span data-ttu-id="10b2b-185">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-185">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="10b2b-186">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="10b2b-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="10b2b-187">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="10b2b-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="10b2b-188">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="10b2b-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="10b2b-189">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="10b2b-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="10b2b-190">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="10b2b-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="10b2b-192">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="10b2b-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="10b2b-193">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="10b2b-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="10b2b-195">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="10b2b-197">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="10b2b-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="10b2b-199">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10b2b-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="10b2b-201">a.</span><span class="sxs-lookup"><span data-stu-id="10b2b-201">a.</span></span> <span data-ttu-id="10b2b-202">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="10b2b-203">b.</span><span class="sxs-lookup"><span data-stu-id="10b2b-203">b.</span></span> <span data-ttu-id="10b2b-204">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="10b2b-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="10b2b-205">c.</span><span class="sxs-lookup"><span data-stu-id="10b2b-205">c.</span></span> <span data-ttu-id="10b2b-206">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="10b2b-207">d.</span><span class="sxs-lookup"><span data-stu-id="10b2b-207">d.</span></span> <span data-ttu-id="10b2b-208">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-208">Click **Create**.</span></span>
 
### <a name="creating-a-thousandeyes-test-user"></a><span data-ttu-id="10b2b-209">Vytvoření zkušebního uživatele ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="10b2b-209">Creating a ThousandEyes test user</span></span>

<span data-ttu-id="10b2b-210">Pokud chcete povolit uživatelům Azure AD přihlášení do ThousandEyes, musí být zřízená do ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="10b2b-210">In order to enable Azure AD users to log into ThousandEyes, they must be provisioned into ThousandEyes.</span></span>  
<span data-ttu-id="10b2b-211">V případě ThousandEyes zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="10b2b-211">In the case of ThousandEyes, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="10b2b-212">Můžete použít všechny ostatní ThousandEyes uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované ThousandEyes zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="10b2b-212">You can use any other ThousandEyes user account creation tools or APIs provided by ThousandEyes to provision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="10b2b-213">**K poskytnutí uživatelského účtu do ThousandEyes, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="10b2b-213">**To provision a user account to ThousandEyes, perform the following steps:**</span></span>

1. <span data-ttu-id="10b2b-214">Přihlaste se k serveru vaší společnosti ThousandEyes jako správce.</span><span class="sxs-lookup"><span data-stu-id="10b2b-214">Log into your ThousandEyes company site as an administrator.</span></span>

2. <span data-ttu-id="10b2b-215">Klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-215">Click **Settings**.</span></span>
   
    <span data-ttu-id="10b2b-216">![Nastavení](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "nastavení")</span><span class="sxs-lookup"><span data-stu-id="10b2b-216">![Settings](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")</span></span>

3. <span data-ttu-id="10b2b-217">Klikněte na tlačítko **účet**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-217">Click **Account**.</span></span>
   
    <span data-ttu-id="10b2b-218">![Účet](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "účtu")</span><span class="sxs-lookup"><span data-stu-id="10b2b-218">![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")</span></span>

4. <span data-ttu-id="10b2b-219">Klikněte **účty a uživatelé** kartě.</span><span class="sxs-lookup"><span data-stu-id="10b2b-219">Click the **Accounts & Users** tab.</span></span>
   
    <span data-ttu-id="10b2b-220">![Účty a uživatelé](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "účty a uživatelé")</span><span class="sxs-lookup"><span data-stu-id="10b2b-220">![Accounts & Users](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")</span></span>

5. <span data-ttu-id="10b2b-221">V **přidat uživatele & účty** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="10b2b-221">In the **Add Users & Accounts** section, perform the following steps:</span></span>
   
    <span data-ttu-id="10b2b-222">![Přidání uživatelských účtů](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "přidejte uživatelské účty")</span><span class="sxs-lookup"><span data-stu-id="10b2b-222">![Add User Accounts](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")</span></span>   
  
    <span data-ttu-id="10b2b-223">a.</span><span class="sxs-lookup"><span data-stu-id="10b2b-223">a.</span></span> <span data-ttu-id="10b2b-224">V **název** textovému poli, zadejte jméno uživatele jako **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-224">In **Name** textbox, type the name of user like **Britta Simon**.</span></span>

    <span data-ttu-id="10b2b-225">b.</span><span class="sxs-lookup"><span data-stu-id="10b2b-225">b.</span></span> <span data-ttu-id="10b2b-226">V **e-mailu** jako typ e-mailu uživatele k textovému poli,  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="10b2b-226">In **Email** textbox, type the email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="10b2b-227">b.</span><span class="sxs-lookup"><span data-stu-id="10b2b-227">b.</span></span> <span data-ttu-id="10b2b-228">Klikněte na tlačítko **přidat nového uživatele k účtu**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-228">Click **Add New User to Account**.</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="10b2b-229">Držitel účtu Azure Active Directory dostane e-mailu, včetně odkaz k potvrzení a aktivovat účet.</span><span class="sxs-lookup"><span data-stu-id="10b2b-229">The Azure Active Directory account holder will get an email including a link to confirm and activate the account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="10b2b-230">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="10b2b-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="10b2b-231">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="10b2b-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ThousandEyes.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="10b2b-233">**Pokud chcete přiřadit Britta Simon ThousandEyes, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="10b2b-233">**To assign Britta Simon to ThousandEyes, perform the following steps:**</span></span>

1. <span data-ttu-id="10b2b-234">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="10b2b-236">V seznamu aplikací vyberte **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-236">In the applications list, select **ThousandEyes**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. <span data-ttu-id="10b2b-238">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="10b2b-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="10b2b-240">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="10b2b-240">Click **Add** button.</span></span> <span data-ttu-id="10b2b-241">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="10b2b-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="10b2b-243">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="10b2b-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="10b2b-244">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="10b2b-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="10b2b-245">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="10b2b-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="10b2b-246">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="10b2b-246">Testing single sign-on</span></span>

<span data-ttu-id="10b2b-247">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="10b2b-247">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="10b2b-248">Když kliknete na dlaždici ThousandEyes na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="10b2b-248">When you click the ThousandEyes tile in the Access Panel, you should get automatically signed-on to your ThousandEyes application.</span></span>

<span data-ttu-id="10b2b-249">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="10b2b-249">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10b2b-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="10b2b-250">Additional resources</span></span>

* [<span data-ttu-id="10b2b-251">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="10b2b-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="10b2b-252">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="10b2b-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

