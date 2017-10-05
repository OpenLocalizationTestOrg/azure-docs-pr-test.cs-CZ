---
title: "Kurz: Integrace Azure Active Directory se základním kamenem OnDemand | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a základním kamenem OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 8bb32588579a0d40b9ae7e0f823c6daab21c856e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a><span data-ttu-id="cc1c8-103">Kurz: Integrace Azure Active Directory se základním kamenem OnDemand</span><span class="sxs-lookup"><span data-stu-id="cc1c8-103">Tutorial: Azure Active Directory integration with Cornerstone OnDemand</span></span>

<span data-ttu-id="cc1c8-104">V tomto kurzu zjistěte, jak integrovat základním kamenem OnDemand s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cc1c8-104">In this tutorial, you learn how to integrate Cornerstone OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cc1c8-105">Integrace základním kamenem OnDemand s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-105">Integrating Cornerstone OnDemand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cc1c8-106">Můžete řídit ve službě Azure AD, který má přístup k základním kamenem OnDemand</span><span class="sxs-lookup"><span data-stu-id="cc1c8-106">You can control in Azure AD who has access to Cornerstone OnDemand</span></span>
- <span data-ttu-id="cc1c8-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k základním kamenem OnDemand (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc1c8-107">You can enable your users to automatically get signed-on to Cornerstone OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cc1c8-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cc1c8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cc1c8-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cc1c8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc1c8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cc1c8-110">Prerequisites</span></span>

<span data-ttu-id="cc1c8-111">Konfigurace integrace Azure AD s základním kamenem OnDemand, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-111">To configure Azure AD integration with Cornerstone OnDemand, you need the following items:</span></span>

- <span data-ttu-id="cc1c8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc1c8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cc1c8-113">Základním kamenem OnDemand jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="cc1c8-113">A Cornerstone OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cc1c8-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cc1c8-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cc1c8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cc1c8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cc1c8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cc1c8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="cc1c8-118">Scenario description</span></span>
<span data-ttu-id="cc1c8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cc1c8-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cc1c8-121">Přidání základním kamenem OnDemand z Galerie</span><span class="sxs-lookup"><span data-stu-id="cc1c8-121">Adding Cornerstone OnDemand from the gallery</span></span>
2. <span data-ttu-id="cc1c8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc1c8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a><span data-ttu-id="cc1c8-123">Přidání základním kamenem OnDemand z Galerie</span><span class="sxs-lookup"><span data-stu-id="cc1c8-123">Adding Cornerstone OnDemand from the gallery</span></span>
<span data-ttu-id="cc1c8-124">Při konfiguraci integrace základním kamenem OnDemand do služby Azure AD potřebujete přidat základním kamenem OnDemand z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-124">To configure the integration of Cornerstone OnDemand into Azure AD, you need to add Cornerstone OnDemand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cc1c8-125">**Pokud chcete přidat základním kamenem OnDemand z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cc1c8-125">**To add Cornerstone OnDemand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cc1c8-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cc1c8-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cc1c8-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="cc1c8-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="cc1c8-133">Do vyhledávacího pole zadejte **základním kamenem OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-133">In the search box, type **Cornerstone OnDemand**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. <span data-ttu-id="cc1c8-135">Na panelu výsledků vyberte **základním kamenem OnDemand**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-135">In the results panel, select **Cornerstone OnDemand**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cc1c8-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc1c8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cc1c8-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se základním kamenem OnDemand podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="cc1c8-138">In this section, you configure and test Azure AD single sign-on with Cornerstone OnDemand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cc1c8-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v základním kamenem OnDemand je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cornerstone OnDemand is to a user in Azure AD.</span></span> <span data-ttu-id="cc1c8-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v základním kamenem OnDemand musí navázat.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-140">In other words, a link relationship between an Azure AD user and the related user in Cornerstone OnDemand needs to be established.</span></span>

<span data-ttu-id="cc1c8-141">V základním kamenem OnDemand přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-141">In Cornerstone OnDemand, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cc1c8-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování se základním kamenem OnDemand, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-142">To configure and test Azure AD single sign-on with Cornerstone OnDemand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cc1c8-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cc1c8-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cc1c8-145">**[Vytvoření zkušebního uživatele základním kamenem OnDemand](#creating-a-cornerstone-ondemand-test-user)**  – Pokud chcete mít protějšek Britta Simon v základním kamenem OnDemand propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-145">**[Creating a Cornerstone OnDemand test user](#creating-a-cornerstone-ondemand-test-user)** - to have a counterpart of Britta Simon in Cornerstone OnDemand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cc1c8-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cc1c8-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cc1c8-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc1c8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cc1c8-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci základním kamenem OnDemand.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cornerstone OnDemand application.</span></span>

<span data-ttu-id="cc1c8-150">**Ke konfiguraci Azure AD jednotné přihlašování se základním kamenem OnDemand, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cc1c8-150">**To configure Azure AD single sign-on with Cornerstone OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="cc1c8-151">Na portálu Azure na **základním kamenem OnDemand** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-151">In the Azure portal, on the **Cornerstone OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="cc1c8-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. <span data-ttu-id="cc1c8-155">Na **základním kamenem OnDemand domény a adresy URL** část, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-155">On the **Cornerstone OnDemand Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    <span data-ttu-id="cc1c8-157">a.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-157">a.</span></span> <span data-ttu-id="cc1c8-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="cc1c8-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.csod.com`</span></span>

    <span data-ttu-id="cc1c8-159">b.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-159">b.</span></span> <span data-ttu-id="cc1c8-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="cc1c8-160">In **Identifier** textbox, type a URL using the following pattern: `https://<company>.csod.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cc1c8-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-161">These values are not real.</span></span> <span data-ttu-id="cc1c8-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cc1c8-163">Obraťte se na [tým podpory základním kamenem OnDemand klienta](mailTo:moreinfo@csod.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-163">Contact [Cornerstone OnDemand Client support team](mailTo:moreinfo@csod.com) to get these values.</span></span> 
 
4. <span data-ttu-id="cc1c8-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. <span data-ttu-id="cc1c8-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cc1c8-168">Na **základním kamenem OnDemand konfigurace** klikněte na tlačítko **konfigurace OnDemand základním kamenem** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-168">On the **Cornerstone OnDemand Configuration** section, click **Configure Cornerstone OnDemand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cc1c8-169">Kopírování **Sign-Out adresu URL a SAML jeden přihlašování služby URL** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="cc1c8-169">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. <span data-ttu-id="cc1c8-171">Konfigurace jednotného přihlašování na **základním kamenem OnDemand** straně, budete muset odeslat stažené **certifikát**, **Sign-Out URL** a **SAML jeden přihlašování adresa URL služby** k [tým podpory základním kamenem OnDemand](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="cc1c8-171">To configure single sign-on on **Cornerstone OnDemand** side, you need to send the downloaded **Certificate**, **Sign-Out URL** and **SAML Single Sign-On Service URL**  to [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span> <span data-ttu-id="cc1c8-172">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="cc1c8-173">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="cc1c8-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cc1c8-174">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cc1c8-175">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cc1c8-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cc1c8-176">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc1c8-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="cc1c8-177">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="cc1c8-179">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cc1c8-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cc1c8-180">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cc1c8-182">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cc1c8-184">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cc1c8-186">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cc1c8-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cc1c8-188">a.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-188">a.</span></span> <span data-ttu-id="cc1c8-189">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cc1c8-190">b.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-190">b.</span></span> <span data-ttu-id="cc1c8-191">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cc1c8-192">c.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-192">c.</span></span> <span data-ttu-id="cc1c8-193">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cc1c8-194">d.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-194">d.</span></span> <span data-ttu-id="cc1c8-195">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-195">Click **Create**.</span></span>
 
### <a name="creating-a-cornerstone-ondemand-test-user"></a><span data-ttu-id="cc1c8-196">Vytvoření zkušebního uživatele základním kamenem OnDemand</span><span class="sxs-lookup"><span data-stu-id="cc1c8-196">Creating a Cornerstone OnDemand test user</span></span>

<span data-ttu-id="cc1c8-197">Pokud chcete povolit uživatelům Azure AD Přihlaste se k základním kamenem OnDemand, musí být zřízená do základním kamenem OnDemand.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-197">In order to enable Azure AD users to log into Cornerstone OnDemand, they must be provisioned into Cornerstone OnDemand.</span></span> <span data-ttu-id="cc1c8-198">V případě základním kamenem OnDemand zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-198">In the case of Cornerstone OnDemand, provisioning is a manual task.</span></span>

<span data-ttu-id="cc1c8-199">Ke konfiguraci zřizování uživatelů, odeslání informací o (například: název, e-mailu) o uživatele Azure AD má účelem zřízení [tým podpory základním kamenem OnDemand](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="cc1c8-199">To configure user provisioning, send the information (e.g.: Name, Email) about the Azure AD user you want to provision to the [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span>

>[!NOTE]
><span data-ttu-id="cc1c8-200">Můžete použít všechny ostatní základním kamenem OnDemand uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované základním kamenem OnDemand zřídit AAD uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-200">You can use any other Cornerstone OnDemand user account creation tools or APIs provided by Cornerstone OnDemand to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cc1c8-201">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc1c8-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cc1c8-202">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k základním kamenem OnDemand.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cornerstone OnDemand.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="cc1c8-204">**Pokud chcete přiřadit Britta Simon základním kamenem OnDemand, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="cc1c8-204">**To assign Britta Simon to Cornerstone OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="cc1c8-205">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="cc1c8-207">V seznamu aplikací vyberte **základním kamenem OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-207">In the applications list, select **Cornerstone OnDemand**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. <span data-ttu-id="cc1c8-209">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="cc1c8-211">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-211">Click **Add** button.</span></span> <span data-ttu-id="cc1c8-212">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="cc1c8-214">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cc1c8-215">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cc1c8-216">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cc1c8-217">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="cc1c8-217">Testing single sign-on</span></span>

<span data-ttu-id="cc1c8-218">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cc1c8-219">Když kliknete na dlaždici základním kamenem OnDemand na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci základním kamenem OnDemand.</span><span class="sxs-lookup"><span data-stu-id="cc1c8-219">When you click the Cornerstone OnDemand tile in the Access Panel, you should get automatically signed-on to your Cornerstone OnDemand application.</span></span>
<span data-ttu-id="cc1c8-220">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cc1c8-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cc1c8-221">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="cc1c8-221">Additional resources</span></span>

* [<span data-ttu-id="cc1c8-222">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc1c8-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cc1c8-223">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cc1c8-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_203.png

