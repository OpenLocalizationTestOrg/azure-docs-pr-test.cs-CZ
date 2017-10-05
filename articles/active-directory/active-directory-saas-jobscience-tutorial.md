---
title: 'Kurz: Azure Active Directory integrace s Jobscience | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jobscience."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 66bec35a8f17482433dbf02827b90620d1cff378
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a><span data-ttu-id="f0b51-103">Kurz: Azure Active Directory integrace s Jobscience</span><span class="sxs-lookup"><span data-stu-id="f0b51-103">Tutorial: Azure Active Directory integration with Jobscience</span></span>

<span data-ttu-id="f0b51-104">V tomto kurzu zjistěte, jak integrovat Jobscience s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f0b51-104">In this tutorial, you learn how to integrate Jobscience with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f0b51-105">Integrace Jobscience s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f0b51-105">Integrating Jobscience with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f0b51-106">Můžete řídit ve službě Azure AD, který má přístup k Jobscience</span><span class="sxs-lookup"><span data-stu-id="f0b51-106">You can control in Azure AD who has access to Jobscience</span></span>
- <span data-ttu-id="f0b51-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Jobscience (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0b51-107">You can enable your users to automatically get signed-on to Jobscience (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f0b51-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f0b51-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f0b51-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f0b51-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f0b51-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f0b51-110">Prerequisites</span></span>

<span data-ttu-id="f0b51-111">Konfigurace integrace Azure AD s Jobscience, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-111">To configure Azure AD integration with Jobscience, you need the following items:</span></span>

- <span data-ttu-id="f0b51-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0b51-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f0b51-113">Jobscience jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f0b51-113">A Jobscience single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f0b51-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0b51-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f0b51-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f0b51-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f0b51-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f0b51-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f0b51-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f0b51-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f0b51-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f0b51-118">Scenario description</span></span>
<span data-ttu-id="f0b51-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f0b51-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f0b51-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f0b51-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f0b51-121">Přidání Jobscience z Galerie</span><span class="sxs-lookup"><span data-stu-id="f0b51-121">Adding Jobscience from the gallery</span></span>
2. <span data-ttu-id="f0b51-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0b51-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobscience-from-the-gallery"></a><span data-ttu-id="f0b51-123">Přidání Jobscience z Galerie</span><span class="sxs-lookup"><span data-stu-id="f0b51-123">Adding Jobscience from the gallery</span></span>
<span data-ttu-id="f0b51-124">Při konfiguraci integrace Jobscience do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Jobscience z galerie.</span><span class="sxs-lookup"><span data-stu-id="f0b51-124">To configure the integration of Jobscience into Azure AD, you need to add Jobscience from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f0b51-125">**Pokud chcete přidat Jobscience z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0b51-125">**To add Jobscience from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f0b51-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f0b51-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f0b51-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f0b51-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f0b51-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0b51-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f0b51-133">Do vyhledávacího pole zadejte **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-133">In the search box, type **Jobscience**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. <span data-ttu-id="f0b51-135">Na panelu výsledků vyberte **Jobscience**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0b51-135">In the results panel, select **Jobscience**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f0b51-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0b51-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f0b51-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobscience podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f0b51-138">In this section, you configure and test Azure AD single sign-on with Jobscience based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f0b51-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Jobscience je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0b51-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jobscience is to a user in Azure AD.</span></span> <span data-ttu-id="f0b51-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Jobscience musí navázat.</span><span class="sxs-lookup"><span data-stu-id="f0b51-140">In other words, a link relationship between an Azure AD user and the related user in Jobscience needs to be established.</span></span>

<span data-ttu-id="f0b51-141">V Jobscience, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="f0b51-141">In Jobscience, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f0b51-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobscience, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-142">To configure and test Azure AD single sign-on with Jobscience, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f0b51-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f0b51-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f0b51-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0b51-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f0b51-145">**[Vytvoření zkušebního uživatele Jobscience](#creating-a-jobscience-test-user)**  – Pokud chcete mít protějšek Britta Simon v Jobscience propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f0b51-145">**[Creating a Jobscience test user](#creating-a-jobscience-test-user)** - to have a counterpart of Britta Simon in Jobscience that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f0b51-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0b51-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f0b51-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f0b51-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f0b51-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0b51-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f0b51-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Jobscience.</span><span class="sxs-lookup"><span data-stu-id="f0b51-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jobscience application.</span></span>

<span data-ttu-id="f0b51-150">**Ke konfiguraci Azure AD jednotné přihlašování s Jobscience, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0b51-150">**To configure Azure AD single sign-on with Jobscience, perform the following steps:**</span></span>

1. <span data-ttu-id="f0b51-151">Na portálu Azure na **Jobscience** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-151">In the Azure portal, on the **Jobscience** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f0b51-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0b51-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. <span data-ttu-id="f0b51-155">Na **Jobscience domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-155">On the **Jobscience Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    <span data-ttu-id="f0b51-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`http://<company name>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="f0b51-157">In the **Sign-on URL** textbox, type a URL using the following pattern:  `http://<company name>.my.salesforce.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="f0b51-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="f0b51-158">This value is not real.</span></span> <span data-ttu-id="f0b51-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f0b51-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="f0b51-160">Tato hodnota získat [tým podpory Jobscience klienta](https://www.jobscience.com/support) nebo z profilu jednotné přihlašování bude vytvořena, což je vysvětleno později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f0b51-160">Get this value by [Jobscience Client support team](https://www.jobscience.com/support) or from the SSO profile you will create which is explained later in the tutorial.</span></span> 
 
4. <span data-ttu-id="f0b51-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="f0b51-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. <span data-ttu-id="f0b51-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0b51-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f0b51-165">Na **Jobscience konfigurace** klikněte na tlačítko **konfigurace Jobscience** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f0b51-165">On the **Jobscience Configuration** section, click **Configure Jobscience** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f0b51-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f0b51-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. <span data-ttu-id="f0b51-168">Přihlaste se k serveru vaší společnosti Jobscience jako správce.</span><span class="sxs-lookup"><span data-stu-id="f0b51-168">Log in to your Jobscience company site as an administrator.</span></span>

8. <span data-ttu-id="f0b51-169">Přejděte na **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-169">Go to **Setup**.</span></span>
   
   <span data-ttu-id="f0b51-170">![Instalační program](./media/active-directory-saas-jobscience-tutorial/IC784358.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="f0b51-170">![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")</span></span>

9. <span data-ttu-id="f0b51-171">V levém navigačním podokně v **Správa** klikněte na tlačítko **Správa domén** rozbalte související část, a potom klikněte na **Moje domény** otevřete **Moje domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="f0b51-171">On the left navigation pane, in the **Administer** section, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
   
   <span data-ttu-id="f0b51-172">![Moje doména](./media/active-directory-saas-jobscience-tutorial/ic767825.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="f0b51-172">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="f0b51-173">Chcete-li ověřit, zda vaše doména byla nastavena správně, ujistěte se, že je v "**krok 4 nasazení uživatelům**" a zkontrolovat vaše "**Moje nastavení domény**".</span><span class="sxs-lookup"><span data-stu-id="f0b51-173">To verify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed to Users**” and review your “**My Domain Settings**”.</span></span>

    <span data-ttu-id="f0b51-174">![Domény uživatele nejsou nasazené](./media/active-directory-saas-jobscience-tutorial/ic784377.png "doménou nasazení pro uživatele")</span><span class="sxs-lookup"><span data-stu-id="f0b51-174">![Domain Deployed to User](./media/active-directory-saas-jobscience-tutorial/ic784377.png "Domain Deployed to User")</span></span>

11. <span data-ttu-id="f0b51-175">Na webu společnosti Jobscience klikněte na tlačítko **ovládacích prvků zabezpečení**a potom klikněte na **nastavení jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-175">On the Jobscience company site, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="f0b51-176">![Ovládací prvky zabezpečení](./media/active-directory-saas-jobscience-tutorial/ic784364.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="f0b51-176">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784364.png "Security Controls")</span></span>

12. <span data-ttu-id="f0b51-177">V **nastavení jednotného přihlašování** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-177">In the **Single Sign-On Settings** section, perform the following steps:</span></span>
    
    <span data-ttu-id="f0b51-178">![Jednotné přihlašování v nastavení](./media/active-directory-saas-jobscience-tutorial/ic781026.png "jednotné přihlašování v nastavení")</span><span class="sxs-lookup"><span data-stu-id="f0b51-178">![Single Sign-On Settings](./media/active-directory-saas-jobscience-tutorial/ic781026.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="f0b51-179">a.</span><span class="sxs-lookup"><span data-stu-id="f0b51-179">a.</span></span> <span data-ttu-id="f0b51-180">Vyberte **povoleno SAML**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-180">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="f0b51-181">b.</span><span class="sxs-lookup"><span data-stu-id="f0b51-181">b.</span></span> <span data-ttu-id="f0b51-182">Klikněte na možnost **Nové**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-182">Click **New**.</span></span>

13. <span data-ttu-id="f0b51-183">Na **SAML jeden přihlašování nastavení upravit** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-183">On the **SAML Single Sign-On Setting Edit** dialog, perform the following steps:</span></span>
    
    <span data-ttu-id="f0b51-184">![SAML jeden přihlašování nastavení](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML jeden přihlašování nastavení")</span><span class="sxs-lookup"><span data-stu-id="f0b51-184">![SAML Single Sign-On Setting](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="f0b51-185">a.</span><span class="sxs-lookup"><span data-stu-id="f0b51-185">a.</span></span> <span data-ttu-id="f0b51-186">V **název** textovému poli, zadejte název pro svou konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="f0b51-186">In the **Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="f0b51-187">b.</span><span class="sxs-lookup"><span data-stu-id="f0b51-187">b.</span></span> <span data-ttu-id="f0b51-188">V **vystavitele** textovému poli, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b51-188">In **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f0b51-189">c.</span><span class="sxs-lookup"><span data-stu-id="f0b51-189">c.</span></span> <span data-ttu-id="f0b51-190">V **Entity Id** textovému poli, typu`https://salesforce-jobscience.com`</span><span class="sxs-lookup"><span data-stu-id="f0b51-190">In the **Entity Id** textbox, type `https://salesforce-jobscience.com`</span></span>

    <span data-ttu-id="f0b51-191">d.</span><span class="sxs-lookup"><span data-stu-id="f0b51-191">d.</span></span> <span data-ttu-id="f0b51-192">Klikněte na tlačítko **Procházet** odeslání vašeho certifikátu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f0b51-192">Click **Browse** to upload your Azure AD certificate.</span></span>

    <span data-ttu-id="f0b51-193">e.</span><span class="sxs-lookup"><span data-stu-id="f0b51-193">e.</span></span> <span data-ttu-id="f0b51-194">Jako **typ Identity SAML**, vyberte **kontrolní výraz obsahuje ID federace z objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-194">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>

    <span data-ttu-id="f0b51-195">f.</span><span class="sxs-lookup"><span data-stu-id="f0b51-195">f.</span></span> <span data-ttu-id="f0b51-196">Jako **umístění Identity SAML**, vyberte **identita je v elementu NameIdentfier příkaz subjektu**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-196">As **SAML Identity Location**, select **Identity is in the NameIdentfier element of the Subject statement**.</span></span>

    <span data-ttu-id="f0b51-197">g.</span><span class="sxs-lookup"><span data-stu-id="f0b51-197">g.</span></span> <span data-ttu-id="f0b51-198">V **adresu URL pro přihlášení zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b51-198">In **Identity Provider Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f0b51-199">h.</span><span class="sxs-lookup"><span data-stu-id="f0b51-199">h.</span></span> <span data-ttu-id="f0b51-200">V **adresa URL odhlašovací zprostředkovatele Identity** textovému poli, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f0b51-200">In **Identity Provider Logout URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="f0b51-201">i.</span><span class="sxs-lookup"><span data-stu-id="f0b51-201">i.</span></span> <span data-ttu-id="f0b51-202">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-202">Click **Save**.</span></span>

14. <span data-ttu-id="f0b51-203">V levém navigačním podokně v **Správa** klikněte na tlačítko **Správa domén** rozbalte související část, a potom klikněte na **Moje domény** otevřete **Moje domény** stránky.</span><span class="sxs-lookup"><span data-stu-id="f0b51-203">On the left navigation pane, in the **Administer** section, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
    
    <span data-ttu-id="f0b51-204">![Moje doména](./media/active-directory-saas-jobscience-tutorial/ic767825.png "Moje doména")</span><span class="sxs-lookup"><span data-stu-id="f0b51-204">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

15. <span data-ttu-id="f0b51-205">Na **Moje domény** stránky v **Branding přihlašovací stránky** klikněte na tlačítko **upravit**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-205">On the **My Domain** page, in the **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="f0b51-206">![Branding přihlašovací stránky](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="f0b51-206">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Login Page Branding")</span></span>

16. <span data-ttu-id="f0b51-207">Na **Branding přihlašovací stránky** stránky v **ověřovací služby** tématu, názvu vaší **nastavení jednotného přihlašování SAML** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f0b51-207">On the **Login Page Branding** page, in the **Authentication Service** section, the name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="f0b51-208">Vyberte ji a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-208">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="f0b51-209">![Branding přihlašovací stránky](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Branding přihlašovací stránky")</span><span class="sxs-lookup"><span data-stu-id="f0b51-209">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Login Page Branding")</span></span>

17. <span data-ttu-id="f0b51-210">Chcete-li získat SP iniciované jednotného přihlašování na klikněte na adresu URL pro přihlášení na **nastavení jednotného přihlašování** v **kontrolních mechanismů pro zabezpečení** části nabídky.</span><span class="sxs-lookup"><span data-stu-id="f0b51-210">To get the SP initiated Single Sign on Login URL click on the **Single Sign On settings** in the **Security Controls** menu section.</span></span>

    <span data-ttu-id="f0b51-211">![Ovládací prvky zabezpečení](./media/active-directory-saas-jobscience-tutorial/ic784368.png "kontrolních mechanismů pro zabezpečení")</span><span class="sxs-lookup"><span data-stu-id="f0b51-211">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784368.png "Security Controls")</span></span>
    
    <span data-ttu-id="f0b51-212">Klikněte na tlačítko jednotného přihlašování k profilu, který jste vytvořili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="f0b51-212">Click the SSO profile you have created in the step above.</span></span> <span data-ttu-id="f0b51-213">Tato stránka zobrazuje jednotného přihlašování na adrese URL vaší společnosti (například [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span><span class="sxs-lookup"><span data-stu-id="f0b51-213">This page shows the Single Sign on URL for your company (for example, [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span></span>    

> [!TIP]
> <span data-ttu-id="f0b51-214">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f0b51-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f0b51-215">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f0b51-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f0b51-216">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f0b51-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f0b51-217">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0b51-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="f0b51-218">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f0b51-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f0b51-220">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0b51-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f0b51-221">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f0b51-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f0b51-223">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f0b51-225">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0b51-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f0b51-227">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f0b51-229">a.</span><span class="sxs-lookup"><span data-stu-id="f0b51-229">a.</span></span> <span data-ttu-id="f0b51-230">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f0b51-231">b.</span><span class="sxs-lookup"><span data-stu-id="f0b51-231">b.</span></span> <span data-ttu-id="f0b51-232">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f0b51-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f0b51-233">c.</span><span class="sxs-lookup"><span data-stu-id="f0b51-233">c.</span></span> <span data-ttu-id="f0b51-234">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f0b51-235">d.</span><span class="sxs-lookup"><span data-stu-id="f0b51-235">d.</span></span> <span data-ttu-id="f0b51-236">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-236">Click **Create**.</span></span>
 
### <a name="creating-a-jobscience-test-user"></a><span data-ttu-id="f0b51-237">Vytvoření zkušebního uživatele Jobscience</span><span class="sxs-lookup"><span data-stu-id="f0b51-237">Creating a Jobscience test user</span></span>

<span data-ttu-id="f0b51-238">Pokud chcete povolit uživatelům Azure AD přihlášení k Jobscience, musí být zřízená do Jobscience.</span><span class="sxs-lookup"><span data-stu-id="f0b51-238">In order to enable Azure AD users to log in to Jobscience, they must be provisioned into Jobscience.</span></span> <span data-ttu-id="f0b51-239">V případě Jobscience zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="f0b51-239">In the case of Jobscience, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="f0b51-240">Můžete použít všechny ostatní Jobscience uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Jobscience zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="f0b51-240">You can use any other Jobscience user account creation tools or APIs provided by Jobscience to provision Azure Active Directory user accounts.</span></span>
>  
        
<span data-ttu-id="f0b51-241">**Pokud chcete konfigurovat, zřizování uživatelů, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0b51-241">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="f0b51-242">Přihlaste se k vaší **Jobscience** společnosti lokality jako správce.</span><span class="sxs-lookup"><span data-stu-id="f0b51-242">Log in to your **Jobscience** company site as administrator.</span></span>

2. <span data-ttu-id="f0b51-243">Přejděte do instalačního programu.</span><span class="sxs-lookup"><span data-stu-id="f0b51-243">Go to Setup.</span></span>
   
   <span data-ttu-id="f0b51-244">![Instalační program](./media/active-directory-saas-jobscience-tutorial/ic784358.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="f0b51-244">![Setup](./media/active-directory-saas-jobscience-tutorial/ic784358.png "Setup")</span></span>
3. <span data-ttu-id="f0b51-245">Přejděte na **Správa uživatelů \> uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-245">Go to **Manage Users \> Users**.</span></span>
   
   <span data-ttu-id="f0b51-246">![Uživatelé](./media/active-directory-saas-jobscience-tutorial/ic784369.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="f0b51-246">![Users](./media/active-directory-saas-jobscience-tutorial/ic784369.png "Users")</span></span>
4. <span data-ttu-id="f0b51-247">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-247">Click **New User**.</span></span>
   
   <span data-ttu-id="f0b51-248">![Všichni uživatelé](./media/active-directory-saas-jobscience-tutorial/ic784370.png "všichni uživatelé")</span><span class="sxs-lookup"><span data-stu-id="f0b51-248">![All Users](./media/active-directory-saas-jobscience-tutorial/ic784370.png "All Users")</span></span>
5. <span data-ttu-id="f0b51-249">Na **upravit uživatele** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f0b51-249">On the **Edit User** dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="f0b51-250">![Úprava uživatele](./media/active-directory-saas-jobscience-tutorial/ic784371.png "Úprava uživatele")</span><span class="sxs-lookup"><span data-stu-id="f0b51-250">![User Edit](./media/active-directory-saas-jobscience-tutorial/ic784371.png "User Edit")</span></span>
   
   <span data-ttu-id="f0b51-251">a.</span><span class="sxs-lookup"><span data-stu-id="f0b51-251">a.</span></span> <span data-ttu-id="f0b51-252">V **křestní jméno** textovému poli, zadejte jméno uživatele, jako je Britta.</span><span class="sxs-lookup"><span data-stu-id="f0b51-252">In the **First Name** textbox, type a first name of the user like Britta.</span></span>
   
   <span data-ttu-id="f0b51-253">b.</span><span class="sxs-lookup"><span data-stu-id="f0b51-253">b.</span></span> <span data-ttu-id="f0b51-254">V **příjmení** textovému poli, zadejte příjmení uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="f0b51-254">In the **Last Name** textbox, type a last name of the user like Simon.</span></span>
   
   <span data-ttu-id="f0b51-255">c.</span><span class="sxs-lookup"><span data-stu-id="f0b51-255">c.</span></span> <span data-ttu-id="f0b51-256">V **Alias** textovému poli, zadejte název aliasu uživatele jako brittas.</span><span class="sxs-lookup"><span data-stu-id="f0b51-256">In the **Alias** textbox, type an alias name of the user like brittas.</span></span>

   <span data-ttu-id="f0b51-257">d.</span><span class="sxs-lookup"><span data-stu-id="f0b51-257">d.</span></span> <span data-ttu-id="f0b51-258">V **e-mailu** jako typ e-mailovou adresu uživatele k textovému poli, Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="f0b51-258">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="f0b51-259">e.</span><span class="sxs-lookup"><span data-stu-id="f0b51-259">e.</span></span> <span data-ttu-id="f0b51-260">V **uživatelské jméno** textové pole, zadejte uživatelské jméno uživatele jako Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="f0b51-260">In the **User Name** textbox, type a user name of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="f0b51-261">f.</span><span class="sxs-lookup"><span data-stu-id="f0b51-261">f.</span></span> <span data-ttu-id="f0b51-262">V **Přezdívka** textovému poli, zadejte název nick uživatele jako Simon.</span><span class="sxs-lookup"><span data-stu-id="f0b51-262">In the **Nick Name** textbox, type a nick name of user like Simon.</span></span>

   <span data-ttu-id="f0b51-263">g.</span><span class="sxs-lookup"><span data-stu-id="f0b51-263">g.</span></span> <span data-ttu-id="f0b51-264">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-264">Click **Save**.</span></span>

    
> [!NOTE]
> <span data-ttu-id="f0b51-265">Držitel účtu Azure Active Directory obdrží e-mailu a dodržuje odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="f0b51-265">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f0b51-266">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f0b51-266">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f0b51-267">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Jobscience.</span><span class="sxs-lookup"><span data-stu-id="f0b51-267">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jobscience.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f0b51-269">**Pokud chcete přiřadit Britta Simon Jobscience, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f0b51-269">**To assign Britta Simon to Jobscience, perform the following steps:**</span></span>

1. <span data-ttu-id="f0b51-270">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-270">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f0b51-272">V seznamu aplikací vyberte **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-272">In the applications list, select **Jobscience**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. <span data-ttu-id="f0b51-274">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f0b51-274">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f0b51-276">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f0b51-276">Click **Add** button.</span></span> <span data-ttu-id="f0b51-277">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0b51-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f0b51-279">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f0b51-279">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f0b51-280">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0b51-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f0b51-281">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f0b51-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f0b51-282">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f0b51-282">Testing single sign-on</span></span>

<span data-ttu-id="f0b51-283">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f0b51-283">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f0b51-284">Když kliknete na dlaždici Jobscience na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Jobscience.</span><span class="sxs-lookup"><span data-stu-id="f0b51-284">When you click the Jobscience tile in the Access Panel, you should get automatically signed-on to your Jobscience application.</span></span>
<span data-ttu-id="f0b51-285">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f0b51-285">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0b51-286">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f0b51-286">Additional resources</span></span>

* [<span data-ttu-id="f0b51-287">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f0b51-287">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f0b51-288">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f0b51-288">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

