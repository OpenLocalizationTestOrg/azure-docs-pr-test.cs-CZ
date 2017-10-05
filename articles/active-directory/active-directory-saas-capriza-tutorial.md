---
title: 'Kurz: Azure Active Directory integrace s platformou Capriza | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Capriza platformu a Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48d92247-f00a-47b9-8d4e-137028d9e200
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 668c094d5330be1c5f71d51d2e76170dc69d1bce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-capriza-platform"></a><span data-ttu-id="5f7da-103">Kurz: Azure Active Directory integrace s Capriza platformy</span><span class="sxs-lookup"><span data-stu-id="5f7da-103">Tutorial: Azure Active Directory integration with Capriza Platform</span></span>

<span data-ttu-id="5f7da-104">V tomto kurzu zjistěte, jak integrovat platformy Capriza s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5f7da-104">In this tutorial, you learn how to integrate Capriza Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5f7da-105">Integrace platformy Capriza s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5f7da-105">Integrating Capriza Platform with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5f7da-106">Můžete řídit ve službě Azure AD, který má přístup k Capriza platformy</span><span class="sxs-lookup"><span data-stu-id="5f7da-106">You can control in Azure AD who has access to Capriza Platform</span></span>
- <span data-ttu-id="5f7da-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Capriza platformy (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f7da-107">You can enable your users to automatically get signed-on to Capriza Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5f7da-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="5f7da-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5f7da-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5f7da-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f7da-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f7da-110">Prerequisites</span></span>

<span data-ttu-id="5f7da-111">Ke konfiguraci integrace služby Azure AD s platformou Capriza, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="5f7da-111">To configure Azure AD integration with Capriza Platform, you need the following items:</span></span>

- <span data-ttu-id="5f7da-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f7da-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5f7da-113">Platformě Capriza jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="5f7da-113">A Capriza Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5f7da-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f7da-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5f7da-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="5f7da-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5f7da-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="5f7da-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5f7da-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f7da-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5f7da-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="5f7da-118">Scenario description</span></span>
<span data-ttu-id="5f7da-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="5f7da-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5f7da-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="5f7da-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5f7da-121">Přidání platformy Capriza z Galerie</span><span class="sxs-lookup"><span data-stu-id="5f7da-121">Adding Capriza Platform from the gallery</span></span>
2. <span data-ttu-id="5f7da-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f7da-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-capriza-platform-from-the-gallery"></a><span data-ttu-id="5f7da-123">Přidání platformy Capriza z Galerie</span><span class="sxs-lookup"><span data-stu-id="5f7da-123">Adding Capriza Platform from the gallery</span></span>
<span data-ttu-id="5f7da-124">Chcete-li konfigurace integrace platformy Capriza do služby Azure AD, přidejte platformu Capriza z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="5f7da-124">To configure the integration of Capriza Platform into Azure AD, you need to add Capriza Platform from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5f7da-125">**Přidat platformy Capriza z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5f7da-125">**To add Capriza Platform from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5f7da-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f7da-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5f7da-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5f7da-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="5f7da-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f7da-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="5f7da-133">Do vyhledávacího pole zadejte **Capriza platformy**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-133">In the search box, type **Capriza Platform**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_search.png)

5. <span data-ttu-id="5f7da-135">Na panelu výsledků vyberte **Capriza platformy**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5f7da-135">In the results panel, select **Capriza Platform**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5f7da-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f7da-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5f7da-138">V této části nakonfigurujete a testovací Azure AD jednotné přihlašování s platformou Capriza podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="5f7da-138">In this section, you configure and test Azure AD single sign-on with Capriza Platform based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5f7da-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v platformě Capriza je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5f7da-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Capriza Platform is to a user in Azure AD.</span></span> <span data-ttu-id="5f7da-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské Capriza platformy je potřeba vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5f7da-140">In other words, a link relationship between an Azure AD user and the related user in Capriza Platform needs to be established.</span></span>

<span data-ttu-id="5f7da-141">V platformě Capriza přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="5f7da-141">In Capriza Platform, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5f7da-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s platformou Capriza, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="5f7da-142">To configure and test Azure AD single sign-on with Capriza Platform, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5f7da-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="5f7da-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5f7da-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f7da-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5f7da-145">**[Vytvoření zkušebního uživatele platformy Capriza](#creating-a-capriza-platform-test-user)**  – Pokud chcete mít protějšek Britta Simon v Capriza platforma, která je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f7da-145">**[Creating a Capriza Platform test user](#creating-a-capriza-platform-test-user)** - to have a counterpart of Britta Simon in Capriza Platform that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5f7da-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f7da-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5f7da-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5f7da-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5f7da-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f7da-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5f7da-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Capriza platformy.</span><span class="sxs-lookup"><span data-stu-id="5f7da-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Capriza Platform application.</span></span>

<span data-ttu-id="5f7da-150">**Ke konfiguraci Azure AD jednotné přihlašování s platformou Capriza, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5f7da-150">**To configure Azure AD single sign-on with Capriza Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="5f7da-151">Na portálu Azure na **Capriza platformy** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-151">In the Azure portal, on the **Capriza Platform** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="5f7da-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f7da-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_samlbase.png)

3. <span data-ttu-id="5f7da-155">Na **Capriza platformy domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f7da-155">On the **Capriza Platform Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_url.png)

    <span data-ttu-id="5f7da-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.capriza.com/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="5f7da-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.capriza.com/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5f7da-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="5f7da-158">This value is not real.</span></span> <span data-ttu-id="5f7da-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="5f7da-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="5f7da-160">Obraťte se na [tým podpory Capriza platformy v klientu](mailTo:support@capriza.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5f7da-160">Contact [Capriza Platform Client support team](mailTo:support@capriza.com) to get this value.</span></span> 

4. <span data-ttu-id="5f7da-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="5f7da-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_certificate.png) 

5. <span data-ttu-id="5f7da-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f7da-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-capriza-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5f7da-165">Na **konfiguraci Platform Capriza** klikněte na tlačítko **konfigurovat platformy Capriza** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="5f7da-165">On the **Capriza Platform Configuration** section, click **Configure Capriza Platform** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5f7da-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="5f7da-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_configure.png) 

7. <span data-ttu-id="5f7da-168">Konfigurace jednotného přihlašování na **Capriza platformy** straně, budete muset odeslat stažený **certifikát**, **Sign-Out URL**, **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** k [tým podpory platformy Capriza](mailTo:support@capriza.com).</span><span class="sxs-lookup"><span data-stu-id="5f7da-168">To configure single sign-on on **Capriza Platform** side, you need to send the downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID** and **SAML Single Sign-On Service URL** to [Capriza Platform support team](mailTo:support@capriza.com).</span></span> <span data-ttu-id="5f7da-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="5f7da-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="5f7da-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="5f7da-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5f7da-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="5f7da-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5f7da-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5f7da-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5f7da-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f7da-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="5f7da-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f7da-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="5f7da-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5f7da-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5f7da-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="5f7da-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5f7da-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5f7da-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f7da-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5f7da-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f7da-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5f7da-185">a.</span><span class="sxs-lookup"><span data-stu-id="5f7da-185">a.</span></span> <span data-ttu-id="5f7da-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5f7da-187">b.</span><span class="sxs-lookup"><span data-stu-id="5f7da-187">b.</span></span> <span data-ttu-id="5f7da-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5f7da-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5f7da-189">c.</span><span class="sxs-lookup"><span data-stu-id="5f7da-189">c.</span></span> <span data-ttu-id="5f7da-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5f7da-191">d.</span><span class="sxs-lookup"><span data-stu-id="5f7da-191">d.</span></span> <span data-ttu-id="5f7da-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-192">Click **Create**.</span></span>
 
### <a name="creating-a-capriza-platform-test-user"></a><span data-ttu-id="5f7da-193">Vytvoření zkušebního uživatele Capriza platformy</span><span class="sxs-lookup"><span data-stu-id="5f7da-193">Creating a Capriza Platform test user</span></span>

<span data-ttu-id="5f7da-194">Cílem této části je vytvoření uživatele v Capriza nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5f7da-194">The objective of this section is to create a user called Britta Simon in Capriza.</span></span> <span data-ttu-id="5f7da-195">Capriza podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="5f7da-195">Capriza supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="5f7da-196">**Ujistěte se, že je pro zřizování uživatelů Capriza nakonfigurován název vaší domény. Potom budou fungovat jenom uživatele za běhu zřizování.**</span><span class="sxs-lookup"><span data-stu-id="5f7da-196">**Please make sure that your domain name is configured with Capriza for user provisioning. After that only the just-in-time user provisioning will work.**</span></span>

<span data-ttu-id="5f7da-197">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="5f7da-197">There is no action item for you in this section.</span></span> <span data-ttu-id="5f7da-198">Vytvoří se nový uživatel během pokusu o přístup k Capriza, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5f7da-198">A new user will be created during an attempt to access Capriza if it doesn't exist yet.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5f7da-199">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="5f7da-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5f7da-200">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k platformě Capriza.</span><span class="sxs-lookup"><span data-stu-id="5f7da-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Capriza Platform.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="5f7da-202">**Pokud chcete přiřadit Britta Simon Capriza platformy, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="5f7da-202">**To assign Britta Simon to Capriza Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="5f7da-203">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="5f7da-205">V seznamu aplikací vyberte **Capriza platformy**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-205">In the applications list, select **Capriza Platform**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_app.png) 

3. <span data-ttu-id="5f7da-207">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="5f7da-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="5f7da-209">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="5f7da-209">Click **Add** button.</span></span> <span data-ttu-id="5f7da-210">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f7da-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="5f7da-212">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="5f7da-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5f7da-213">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f7da-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5f7da-214">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="5f7da-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5f7da-215">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="5f7da-215">Testing single sign-on</span></span>

<span data-ttu-id="5f7da-216">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="5f7da-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5f7da-217">Když kliknete na dlaždici Capriza platformy na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Capriza.</span><span class="sxs-lookup"><span data-stu-id="5f7da-217">When you click the Capriza Platform tile in the Access Panel, you should get automatically signed-on to your Capriza application.</span></span> <span data-ttu-id="5f7da-218">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5f7da-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5f7da-219">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="5f7da-219">Additional resources</span></span>

* [<span data-ttu-id="5f7da-220">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5f7da-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5f7da-221">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5f7da-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_203.png

