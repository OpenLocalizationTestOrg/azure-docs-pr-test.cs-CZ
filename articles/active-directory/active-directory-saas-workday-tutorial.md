---
title: 'Kurz: Azure Active Directory integraci s Workday | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a během pracovního dne."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 164d5c644f120fa86e2b690649241892764b64b7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a><span data-ttu-id="e5d99-103">Kurz: Azure Active Directory integraci s Workday</span><span class="sxs-lookup"><span data-stu-id="e5d99-103">Tutorial: Azure Active Directory integration with Workday</span></span>

<span data-ttu-id="e5d99-104">V tomto kurzu zjistěte, jak integrovat Workday s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e5d99-104">In this tutorial, you learn how to integrate Workday with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e5d99-105">Integrace Workday s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="e5d99-105">Integrating Workday with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e5d99-106">Můžete řídit ve službě Azure AD, který má přístup k Workday</span><span class="sxs-lookup"><span data-stu-id="e5d99-106">You can control in Azure AD who has access to Workday</span></span>
- <span data-ttu-id="e5d99-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Workday (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5d99-107">You can enable your users to automatically get signed-on to Workday (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e5d99-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="e5d99-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e5d99-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e5d99-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5d99-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e5d99-110">Prerequisites</span></span>

<span data-ttu-id="e5d99-111">Chcete-li konfigurovat Azure AD integraci s Workday, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-111">To configure Azure AD integration with Workday, you need the following items:</span></span>

- <span data-ttu-id="e5d99-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5d99-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e5d99-113">Workday jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="e5d99-113">A Workday single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e5d99-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="e5d99-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e5d99-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="e5d99-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e5d99-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="e5d99-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e5d99-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e5d99-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e5d99-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="e5d99-118">Scenario description</span></span>
<span data-ttu-id="e5d99-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="e5d99-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e5d99-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="e5d99-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e5d99-121">Přidání Workday z Galerie</span><span class="sxs-lookup"><span data-stu-id="e5d99-121">Adding Workday from the gallery</span></span>
2. <span data-ttu-id="e5d99-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5d99-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workday-from-the-gallery"></a><span data-ttu-id="e5d99-123">Přidání Workday z Galerie</span><span class="sxs-lookup"><span data-stu-id="e5d99-123">Adding Workday from the gallery</span></span>
<span data-ttu-id="e5d99-124">Konfigurace integrace Workday do Azure AD, potřebujete přidat Workday z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="e5d99-124">To configure the integration of Workday into Azure AD, you need to add Workday from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e5d99-125">**Pokud chcete přidat Workday z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e5d99-125">**To add Workday from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e5d99-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e5d99-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e5d99-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e5d99-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="e5d99-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5d99-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="e5d99-133">Do vyhledávacího pole zadejte **Workday**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-133">In the search box, type **Workday**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. <span data-ttu-id="e5d99-135">Na panelu výsledků vyberte **Workday**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e5d99-135">In the results panel, select **Workday**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e5d99-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5d99-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e5d99-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workday podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="e5d99-138">In this section, you configure and test Azure AD single sign-on with Workday based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e5d99-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Workday je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e5d99-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workday is to a user in Azure AD.</span></span> <span data-ttu-id="e5d99-140">Jinými slovy musí odkaz vztah mezi uživatele Azure AD a související uživatelské ve Workday zřídit.</span><span class="sxs-lookup"><span data-stu-id="e5d99-140">In other words, a link relationship between an Azure AD user and the related user in Workday needs to be established.</span></span>

<span data-ttu-id="e5d99-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** ve Workday.</span><span class="sxs-lookup"><span data-stu-id="e5d99-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workday.</span></span>

<span data-ttu-id="e5d99-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Workday, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-142">To configure and test Azure AD single sign-on with Workday, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e5d99-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="e5d99-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e5d99-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e5d99-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e5d99-145">**[Vytvoření zkušebního uživatele Workday](#creating-a-workday-test-user)**  – Pokud chcete mít protějšek Britta Simon ve Workday propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5d99-145">**[Creating a Workday test user](#creating-a-workday-test-user)** - to have a counterpart of Britta Simon in Workday that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e5d99-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e5d99-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e5d99-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e5d99-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e5d99-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5d99-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e5d99-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci během pracovního dne.</span><span class="sxs-lookup"><span data-stu-id="e5d99-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workday application.</span></span>

<span data-ttu-id="e5d99-150">**Ke konfiguraci Azure AD jednotné přihlašování s Workday, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e5d99-150">**To configure Azure AD single sign-on with Workday, perform the following steps:**</span></span>

1. <span data-ttu-id="e5d99-151">Na portálu Azure na **Workday** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-151">In the Azure portal, on the **Workday** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="e5d99-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="e5d99-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. <span data-ttu-id="e5d99-155">Na **Workday domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-155">On the **Workday Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    <span data-ttu-id="e5d99-157">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-157">a.</span></span> <span data-ttu-id="e5d99-158">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu jako:`https://impl.workday.com/<tenant>/login-saml2.htmld`</span><span class="sxs-lookup"><span data-stu-id="e5d99-158">In the **Sign-on URL** textbox, type the value as: `https://impl.workday.com/<tenant>/login-saml2.htmld`</span></span>

    <span data-ttu-id="e5d99-159">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-159">b.</span></span> <span data-ttu-id="e5d99-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://impl.workday.com/<tenant>/login-saml.htmld`</span><span class="sxs-lookup"><span data-stu-id="e5d99-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://impl.workday.com/<tenant>/login-saml.htmld`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e5d99-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="e5d99-161">These values are not the real.</span></span> <span data-ttu-id="e5d99-162">Tyto hodnoty aktualizujte skutečná adresa URL přihlašování a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="e5d99-162">Update these values with the actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="e5d99-163">Vaše adresa URL odpovědi musí mít například subdoména: www, wd2, wd3, wd3 impl, wd5, wd5 impl).</span><span class="sxs-lookup"><span data-stu-id="e5d99-163">Your reply URL must have a subdomain for example: www, wd2, wd3, wd3-impl, wd5, wd5-impl).</span></span> <span data-ttu-id="e5d99-164">Pomocí něco podobného jako "*http://www.myworkday.com*" funguje, ale "*http://myworkday.com*" neexistuje.</span><span class="sxs-lookup"><span data-stu-id="e5d99-164">Using something like "*http://www.myworkday.com*" works but "*http://myworkday.com*" does not.</span></span> <span data-ttu-id="e5d99-165">Obraťte se na [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="e5d99-165">Contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) to get these values.</span></span> 
 

4. <span data-ttu-id="e5d99-166">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="e5d99-166">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. <span data-ttu-id="e5d99-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5d99-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e5d99-170">Na **Workday konfigurace** klikněte na tlačítko **konfigurace Workday** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="e5d99-170">On the **Workday Configuration** section, click **Configure Workday** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e5d99-171">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="e5d99-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="e5d99-172">![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="e5d99-172">![Configure Single Sign-On](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span></span>
7. <span data-ttu-id="e5d99-173">V okně prohlížeče jiný web Přihlaste se k serveru vaší společnosti Workday jako správce.</span><span class="sxs-lookup"><span data-stu-id="e5d99-173">In a different web browser window, log in to your Workday company site as an administrator.</span></span>

8. <span data-ttu-id="e5d99-174">Přejděte na **nabídky \> Workbench**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-174">Go to **Menu \> Workbench**.</span></span>
   
    <span data-ttu-id="e5d99-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span><span class="sxs-lookup"><span data-stu-id="e5d99-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span></span>

9. <span data-ttu-id="e5d99-176">Přejděte na **účet správy**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-176">Go to **Account Administration**.</span></span>
   
    <span data-ttu-id="e5d99-177">![Účet správy](./media/active-directory-saas-workday-tutorial/IC782924.png "účet správy")</span><span class="sxs-lookup"><span data-stu-id="e5d99-177">![Account Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")</span></span>

10. <span data-ttu-id="e5d99-178">Přejděte na **upravit nastavení klienta – zabezpečení**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-178">Go to **Edit Tenant Setup – Security**.</span></span>
   
    <span data-ttu-id="e5d99-179">![Upravit zabezpečení klienta](./media/active-directory-saas-workday-tutorial/IC782925.png "upravit klienta zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="e5d99-179">![Edit Tenant Security](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")</span></span>

11. <span data-ttu-id="e5d99-180">V **adresy URL pro přesměrování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-180">In the **Redirection URLs** section, perform the following steps:</span></span>
   
    <span data-ttu-id="e5d99-181">![Adresy URL pro přesměrování](./media/active-directory-saas-workday-tutorial/IC7829581.png "adresy URL pro přesměrování")</span><span class="sxs-lookup"><span data-stu-id="e5d99-181">![Redirection URLs](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")</span></span>
   
    <span data-ttu-id="e5d99-182">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-182">a.</span></span> <span data-ttu-id="e5d99-183">Klikněte na tlačítko **přidejte řádek**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-183">Click **Add Row**.</span></span>
   
    <span data-ttu-id="e5d99-184">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-184">b.</span></span> <span data-ttu-id="e5d99-185">V **adresy URL přesměrování při přihlášení** textové pole a **adresu URL pro přesměrování mobilní** textovému poli, typ **přihlašovací adresa URL** jste zadali na **Workday domény a adresy URL** části portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="e5d99-185">In the **Login Redirect URL** textbox and the **Mobile Redirect URL** textbox, type the **Sign-on URL** you have entered on the **Workday Domain and URLs** section of the Azure portal.</span></span>
   
    <span data-ttu-id="e5d99-186">c.</span><span class="sxs-lookup"><span data-stu-id="e5d99-186">c.</span></span> <span data-ttu-id="e5d99-187">Na portálu Azure na **konfigurovat přihlášení** okno, kopie **Sign-Out URL**a vložte ji do **adresy URL přesměrování odhlašovací** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e5d99-187">In the Azure portal, on the **Configure sign-on** window, copy the **Sign-Out URL**, and then paste it into the **Logout Redirect URL** textbox.</span></span>
   
    <span data-ttu-id="e5d99-188">d.</span><span class="sxs-lookup"><span data-stu-id="e5d99-188">d.</span></span>  <span data-ttu-id="e5d99-189">V **prostředí** textovému poli, zadejte název prostředí.</span><span class="sxs-lookup"><span data-stu-id="e5d99-189">In **Environment** textbox, type the environment name.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="e5d99-190">Hodnota atributu prostředí je vázaný na hodnotu adresy URL klienta:</span><span class="sxs-lookup"><span data-stu-id="e5d99-190">The value of the Environment attribute is tied to the value of the tenant URL:</span></span>  
    ><span data-ttu-id="e5d99-191">-Pokud název domény adresy URL klienta Workday začíná impl například: *https://impl.workday.com/\<klienta\>/login-saml2.htmld*), **prostředí** musí být nastaven pro implementaci.</span><span class="sxs-lookup"><span data-stu-id="e5d99-191">-If the domain name of the Workday tenant URL starts with impl for example: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), the **Environment** attribute must be set to Implementation.</span></span>  
    ><span data-ttu-id="e5d99-192">– Pokud je název domény začíná něco jiného, budete muset kontaktovat [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) získat shody **prostředí** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e5d99-192">-If the domain name starts with something else, you need to contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) to get the matching **Environment** value.</span></span>

12. <span data-ttu-id="e5d99-193">V **SAML instalace** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-193">In the **SAML Setup** section, perform the following steps:</span></span>
   
    <span data-ttu-id="e5d99-194">![Instalační program SAML](./media/active-directory-saas-workday-tutorial/IC782926.png "instalace SAML")</span><span class="sxs-lookup"><span data-stu-id="e5d99-194">![SAML Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")</span></span>
   
    <span data-ttu-id="e5d99-195">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-195">a.</span></span>  <span data-ttu-id="e5d99-196">Vyberte **povolit ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-196">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="e5d99-197">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-197">b.</span></span>  <span data-ttu-id="e5d99-198">Klikněte na tlačítko **přidejte řádek**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-198">Click **Add Row**.</span></span>

13. <span data-ttu-id="e5d99-199">V části zprostředkovatelů Identity SAML proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-199">In the SAML Identity Providers section, perform the following steps:</span></span>
   
    <span data-ttu-id="e5d99-200">![Zprostředkovatelé Identity SAML](./media/active-directory-saas-workday-tutorial/IC7829271.png "zprostředkovatelů Identity SAML")</span><span class="sxs-lookup"><span data-stu-id="e5d99-200">![SAML Identity Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")</span></span>
   
    <span data-ttu-id="e5d99-201">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-201">a.</span></span> <span data-ttu-id="e5d99-202">Do textového pole Název zprostředkovatele Identity, zadejte název zprostředkovatele (například: *SPInitiatedSSO*).</span><span class="sxs-lookup"><span data-stu-id="e5d99-202">In the Identity Provider Name textbox, type a provider name (for example: *SPInitiatedSSO*).</span></span>
   
    <span data-ttu-id="e5d99-203">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-203">b.</span></span> <span data-ttu-id="e5d99-204">Na portálu Azure na **konfigurovat přihlášení** okno, kopie **SAML Entity ID** hodnotu a vložte ji do **vystavitele** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e5d99-204">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Entity ID** value, and then paste it into the **Issuer** textbox.</span></span>
   
    <span data-ttu-id="e5d99-205">c.</span><span class="sxs-lookup"><span data-stu-id="e5d99-205">c.</span></span> <span data-ttu-id="e5d99-206">Vyberte **povolit Workday Initiated odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-206">Select **Enable Workday Initiated Logout**.</span></span>
   
    <span data-ttu-id="e5d99-207">d.</span><span class="sxs-lookup"><span data-stu-id="e5d99-207">d.</span></span> <span data-ttu-id="e5d99-208">Na portálu Azure na **konfigurovat přihlášení** okno, kopie **Sign-Out URL** hodnotu a vložte ji do **adresy URL žádosti o odhlášení** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e5d99-208">In the Azure portal, on the **Configure sign-on** window, copy the **Sign-Out URL** value, and then paste it into the **Logout Request URL** textbox.</span></span>

    <span data-ttu-id="e5d99-209">e.</span><span class="sxs-lookup"><span data-stu-id="e5d99-209">e.</span></span> <span data-ttu-id="e5d99-210">Klikněte na tlačítko **certifikátu veřejného klíče zprostředkovatele Identity**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-210">Click **Identity Provider Public Key Certificate**, and then click **Create**.</span></span> 

    <span data-ttu-id="e5d99-211">![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782928.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="e5d99-211">![Create](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")</span></span>

    <span data-ttu-id="e5d99-212">f.</span><span class="sxs-lookup"><span data-stu-id="e5d99-212">f.</span></span> <span data-ttu-id="e5d99-213">Klikněte na tlačítko **vytvořit x509 veřejný klíč**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-213">Click **Create x509 Public Key**.</span></span> 

    <span data-ttu-id="e5d99-214">![Vytvoření](./media/active-directory-saas-workday-tutorial/IC782929.png "vytvořit")</span><span class="sxs-lookup"><span data-stu-id="e5d99-214">![Create](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")</span></span>


14. <span data-ttu-id="e5d99-215">V **veřejný klíč zobrazení x509** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-215">In the **View x509 Public Key** section, perform the following steps:</span></span> 
   
    <span data-ttu-id="e5d99-216">![Veřejný klíč zobrazení x509](./media/active-directory-saas-workday-tutorial/IC782930.png "zobrazení x509 veřejný klíč")</span><span class="sxs-lookup"><span data-stu-id="e5d99-216">![View x509 Public Key](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key")</span></span> 
   
    <span data-ttu-id="e5d99-217">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-217">a.</span></span> <span data-ttu-id="e5d99-218">V **název** textovému poli, zadejte název pro svůj certifikát (například: *PPE\_SP*).</span><span class="sxs-lookup"><span data-stu-id="e5d99-218">In the **Name** textbox, type a name for your certificate (for example: *PPE\_SP*).</span></span>
   
    <span data-ttu-id="e5d99-219">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-219">b.</span></span> <span data-ttu-id="e5d99-220">V **platný z** textovému poli, zadejte platný z hodnota atributu vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e5d99-220">In the **Valid From** textbox, type the valid from attribute value of your certificate.</span></span>
   
    <span data-ttu-id="e5d99-221">c.</span><span class="sxs-lookup"><span data-stu-id="e5d99-221">c.</span></span>  <span data-ttu-id="e5d99-222">V **platný k** textovému poli, zadejte platnou hodnotu atributu vašeho certifikátu.</span><span class="sxs-lookup"><span data-stu-id="e5d99-222">In the **Valid To** textbox, type the valid to attribute value of your certificate.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="e5d99-223">Můžete získat od data a platném datum, od stažený certifikát poklepáním platný.</span><span class="sxs-lookup"><span data-stu-id="e5d99-223">You can get the valid from date and the valid to date from the downloaded certificate by double-clicking it.</span></span>  <span data-ttu-id="e5d99-224">Kalendářní data jsou uvedeny v seznamu **podrobnosti** kartě.</span><span class="sxs-lookup"><span data-stu-id="e5d99-224">The dates are listed under the **Details** tab.</span></span>
    > 
    >
   
    <span data-ttu-id="e5d99-225">d.</span><span class="sxs-lookup"><span data-stu-id="e5d99-225">d.</span></span>  <span data-ttu-id="e5d99-226">Otevření kódovaného certifikátu kódování base-64 v programu Poznámkový blok a zkopírujte obsah ho.</span><span class="sxs-lookup"><span data-stu-id="e5d99-226">Open your base-64 encoded certificate in notepad, and then copy the content of it.</span></span>
   
    <span data-ttu-id="e5d99-227">e.</span><span class="sxs-lookup"><span data-stu-id="e5d99-227">e.</span></span>  <span data-ttu-id="e5d99-228">V **certifikát** textovému poli, vložit obsah schránky.</span><span class="sxs-lookup"><span data-stu-id="e5d99-228">In the **Certificate** textbox, paste the content of your clipboard.</span></span>
   
    <span data-ttu-id="e5d99-229">f.</span><span class="sxs-lookup"><span data-stu-id="e5d99-229">f.</span></span>  <span data-ttu-id="e5d99-230">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-230">Click **OK**.</span></span>

15. <span data-ttu-id="e5d99-231">Proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-231">Perform the following steps:</span></span> 
   
    <span data-ttu-id="e5d99-232">![Konfigurace jednotného přihlašování k](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "Konfigurace jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="e5d99-232">![SSO configuration](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO configuration")</span></span>
   
    <span data-ttu-id="e5d99-233">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-233">a.</span></span>  <span data-ttu-id="e5d99-234">Povolit **x509 pár privátního klíče**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-234">Enable the **x509 Private Key Pair**.</span></span>
   
    <span data-ttu-id="e5d99-235">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-235">b.</span></span>  <span data-ttu-id="e5d99-236">V **ID zprostředkovatele služby** textovému poli, typ **http://www.workday.com**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-236">In the **Service Provider ID** textbox, type **http://www.workday.com**.</span></span>
   
    <span data-ttu-id="e5d99-237">c.</span><span class="sxs-lookup"><span data-stu-id="e5d99-237">c.</span></span>  <span data-ttu-id="e5d99-238">Vyberte **povolit SP spustil ověřování SAML**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-238">Select **Enable SP Initiated SAML Authentication**.</span></span>
   
    <span data-ttu-id="e5d99-239">d.</span><span class="sxs-lookup"><span data-stu-id="e5d99-239">d.</span></span>  <span data-ttu-id="e5d99-240">Na portálu Azure na **konfigurovat přihlášení** okno, kopie **SAML jeden přihlašování adresa URL služby** hodnotu a vložte ji do **IdP adresa URL služby jednotného přihlašování k** textové pole.</span><span class="sxs-lookup"><span data-stu-id="e5d99-240">In the Azure portal, on the **Configure sign-on** window, copy the **SAML Single Sign-On Service URL** value, and then paste it into the **IdP SSO Service URL** textbox.</span></span>
   
    <span data-ttu-id="e5d99-241">e.</span><span class="sxs-lookup"><span data-stu-id="e5d99-241">e.</span></span> <span data-ttu-id="e5d99-242">Vyberte **není Deflate žádosti o ověření spouštěná SP**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-242">Select **Do Not Deflate SP-initiated Authentication Request**.</span></span>
   
    <span data-ttu-id="e5d99-243">f.</span><span class="sxs-lookup"><span data-stu-id="e5d99-243">f.</span></span> <span data-ttu-id="e5d99-244">Jako **metoda žádosti o podpis ověřování**, vyberte **SHA256**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-244">As **Authentication Request Signature Method**, select **SHA256**.</span></span> 
   
    <span data-ttu-id="e5d99-245">![Metoda ověřování žádost o podpis](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "metodu ověřování žádost o podpis")</span><span class="sxs-lookup"><span data-stu-id="e5d99-245">![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "Authentication Request Signature Method")</span></span> 
   
    <span data-ttu-id="e5d99-246">g.</span><span class="sxs-lookup"><span data-stu-id="e5d99-246">g.</span></span> <span data-ttu-id="e5d99-247">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-247">Click **OK**.</span></span> 
   
    <span data-ttu-id="e5d99-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span><span class="sxs-lookup"><span data-stu-id="e5d99-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span></span>

> [!TIP]
> <span data-ttu-id="e5d99-249">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="e5d99-249">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e5d99-250">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="e5d99-250">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e5d99-251">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e5d99-251">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e5d99-252">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5d99-252">Creating an Azure AD test user</span></span>
<span data-ttu-id="e5d99-253">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e5d99-253">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="e5d99-255">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e5d99-255">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e5d99-256">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="e5d99-256">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e5d99-258">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-258">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e5d99-260">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5d99-260">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e5d99-262">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="e5d99-262">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e5d99-264">a.</span><span class="sxs-lookup"><span data-stu-id="e5d99-264">a.</span></span> <span data-ttu-id="e5d99-265">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-265">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e5d99-266">b.</span><span class="sxs-lookup"><span data-stu-id="e5d99-266">b.</span></span> <span data-ttu-id="e5d99-267">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e5d99-267">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e5d99-268">c.</span><span class="sxs-lookup"><span data-stu-id="e5d99-268">c.</span></span> <span data-ttu-id="e5d99-269">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-269">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e5d99-270">d.</span><span class="sxs-lookup"><span data-stu-id="e5d99-270">d.</span></span> <span data-ttu-id="e5d99-271">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-271">Click **Create**.</span></span>
 
### <a name="creating-a-workday-test-user"></a><span data-ttu-id="e5d99-272">Vytvoření zkušebního uživatele Workday</span><span class="sxs-lookup"><span data-stu-id="e5d99-272">Creating a Workday test user</span></span>

<span data-ttu-id="e5d99-273">Pokud chcete získat testovacího uživatele zřízené do Workday, budete muset kontaktovat [tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html).</span><span class="sxs-lookup"><span data-stu-id="e5d99-273">To get a test user provisioned into Workday, you need to contact the [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html).</span></span>
<span data-ttu-id="e5d99-274">[Tým podpory klienta Workday](https://www.workday.com/en-us/partners-services/services/support.html) pro vás vytvoří uživatele.</span><span class="sxs-lookup"><span data-stu-id="e5d99-274">The [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) creates the user for you.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e5d99-275">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="e5d99-275">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e5d99-276">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k Workday.</span><span class="sxs-lookup"><span data-stu-id="e5d99-276">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workday.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="e5d99-278">**Pokud chcete přiřadit Britta Simon Workday, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="e5d99-278">**To assign Britta Simon to Workday, perform the following steps:**</span></span>

1. <span data-ttu-id="e5d99-279">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-279">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="e5d99-281">V seznamu aplikací vyberte **Workday**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-281">In the applications list, select **Workday**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. <span data-ttu-id="e5d99-283">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="e5d99-283">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="e5d99-285">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5d99-285">Click **Add** button.</span></span> <span data-ttu-id="e5d99-286">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5d99-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="e5d99-288">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="e5d99-288">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e5d99-289">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5d99-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e5d99-290">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="e5d99-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e5d99-291">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="e5d99-291">Testing single sign-on</span></span>

<span data-ttu-id="e5d99-292">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="e5d99-292">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="e5d99-293">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e5d99-293">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5d99-294">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e5d99-294">Additional resources</span></span>

* [<span data-ttu-id="e5d99-295">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e5d99-295">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e5d99-296">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e5d99-296">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="e5d99-297">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="e5d99-297">Configure User Provisioning</span></span>](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

