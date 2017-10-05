---
title: 'Kurz: Azure Active Directory integrace s Jobbadmin | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jobbadmin."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c5208b0d-66a3-49ed-9aad-70d21f54aee0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 848a6d6d0f072bc3f697ff57756714fc45e7dcc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobbadmin"></a><span data-ttu-id="f5a0f-103">Kurz: Azure Active Directory integrace s Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="f5a0f-103">Tutorial: Azure Active Directory integration with Jobbadmin</span></span>

<span data-ttu-id="f5a0f-104">V tomto kurzu zjistěte, jak integrovat Jobbadmin s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f5a0f-104">In this tutorial, you learn how to integrate Jobbadmin with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f5a0f-105">Integrace Jobbadmin s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-105">Integrating Jobbadmin with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f5a0f-106">Můžete řídit ve službě Azure AD, který má přístup k Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="f5a0f-106">You can control in Azure AD who has access to Jobbadmin</span></span>
- <span data-ttu-id="f5a0f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Jobbadmin (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5a0f-107">You can enable your users to automatically get signed-on to Jobbadmin (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f5a0f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f5a0f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f5a0f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f5a0f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5a0f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f5a0f-110">Prerequisites</span></span>

<span data-ttu-id="f5a0f-111">Konfigurace integrace Azure AD s Jobbadmin, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-111">To configure Azure AD integration with Jobbadmin, you need the following items:</span></span>

- <span data-ttu-id="f5a0f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5a0f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f5a0f-113">Jobbadmin jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f5a0f-113">A Jobbadmin single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f5a0f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f5a0f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f5a0f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f5a0f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5a0f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f5a0f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f5a0f-118">Scenario description</span></span>
<span data-ttu-id="f5a0f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f5a0f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f5a0f-121">Přidání Jobbadmin z Galerie</span><span class="sxs-lookup"><span data-stu-id="f5a0f-121">Adding Jobbadmin from the gallery</span></span>
2. <span data-ttu-id="f5a0f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5a0f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobbadmin-from-the-gallery"></a><span data-ttu-id="f5a0f-123">Přidání Jobbadmin z Galerie</span><span class="sxs-lookup"><span data-stu-id="f5a0f-123">Adding Jobbadmin from the gallery</span></span>
<span data-ttu-id="f5a0f-124">Při konfiguraci integrace Jobbadmin do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Jobbadmin z galerie.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-124">To configure the integration of Jobbadmin into Azure AD, you need to add Jobbadmin from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f5a0f-125">**Pokud chcete přidat Jobbadmin z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f5a0f-125">**To add Jobbadmin from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a0f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f5a0f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f5a0f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f5a0f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f5a0f-133">Do vyhledávacího pole zadejte **Jobbadmin**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-133">In the search box, type **Jobbadmin**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_search.png)

5. <span data-ttu-id="f5a0f-135">Na panelu výsledků vyberte **Jobbadmin**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-135">In the results panel, select **Jobbadmin**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f5a0f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5a0f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f5a0f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobbadmin podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f5a0f-138">In this section, you configure and test Azure AD single sign-on with Jobbadmin based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f5a0f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Jobbadmin je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jobbadmin is to a user in Azure AD.</span></span> <span data-ttu-id="f5a0f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Jobbadmin musí navázat.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-140">In other words, a link relationship between an Azure AD user and the related user in Jobbadmin needs to be established.</span></span>

<span data-ttu-id="f5a0f-141">V Jobbadmin, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-141">In Jobbadmin, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f5a0f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jobbadmin, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-142">To configure and test Azure AD single sign-on with Jobbadmin, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f5a0f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f5a0f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f5a0f-145">**[Vytvoření zkušebního uživatele Jobbadmin](#creating-a-jobbadmin-test-user)**  – Pokud chcete mít protějšek Britta Simon v Jobbadmin propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-145">**[Creating a Jobbadmin test user](#creating-a-jobbadmin-test-user)** - to have a counterpart of Britta Simon in Jobbadmin that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f5a0f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f5a0f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f5a0f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5a0f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f5a0f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Jobbadmin.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jobbadmin application.</span></span>

<span data-ttu-id="f5a0f-150">**Ke konfiguraci Azure AD jednotné přihlašování s Jobbadmin, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f5a0f-150">**To configure Azure AD single sign-on with Jobbadmin, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a0f-151">Na portálu Azure na **Jobbadmin** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-151">In the Azure portal, on the **Jobbadmin** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f5a0f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_samlbase.png)

3. <span data-ttu-id="f5a0f-155">Na **Jobbadmin domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-155">On the **Jobbadmin Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_url.png)

    <span data-ttu-id="f5a0f-157">a.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-157">a.</span></span> <span data-ttu-id="f5a0f-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span><span class="sxs-lookup"><span data-stu-id="f5a0f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span></span>

    <span data-ttu-id="f5a0f-159">b.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-159">b.</span></span> <span data-ttu-id="f5a0f-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.jobnorge.no`</span><span class="sxs-lookup"><span data-stu-id="f5a0f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.jobnorge.no`</span></span>

    <span data-ttu-id="f5a0f-161">c.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-161">c.</span></span> <span data-ttu-id="f5a0f-162">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span><span class="sxs-lookup"><span data-stu-id="f5a0f-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<instancename>.jobbnorge.no/auth/saml2/login.ashx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f5a0f-163">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-163">These values are not real.</span></span> <span data-ttu-id="f5a0f-164">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-164">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f5a0f-165">Obraťte se na [tým podpory Jobbadmin klienta](https://www.jobbnorge.no/om-oss/kontakt-oss) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-165">Contact [Jobbadmin Client support team](https://www.jobbnorge.no/om-oss/kontakt-oss) to get these values.</span></span> 
 


4. <span data-ttu-id="f5a0f-166">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_certificate.png) 

5. <span data-ttu-id="f5a0f-168">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-168">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f5a0f-170">Konfigurace jednotného přihlašování na **Jobbadmin** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Jobbadmin](https://www.jobbnorge.no/om-oss/kontakt-oss).</span><span class="sxs-lookup"><span data-stu-id="f5a0f-170">To configure single sign-on on **Jobbadmin** side, you need to send the downloaded **Metadata XML** to [Jobbadmin support team](https://www.jobbnorge.no/om-oss/kontakt-oss).</span></span> <span data-ttu-id="f5a0f-171">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-171">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f5a0f-172">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f5a0f-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f5a0f-173">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f5a0f-174">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f5a0f-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f5a0f-175">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5a0f-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="f5a0f-176">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f5a0f-178">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f5a0f-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a0f-179">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f5a0f-181">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f5a0f-183">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f5a0f-185">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5a0f-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-jobbadmin-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f5a0f-187">a.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-187">a.</span></span> <span data-ttu-id="f5a0f-188">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f5a0f-189">b.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-189">b.</span></span> <span data-ttu-id="f5a0f-190">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f5a0f-191">c.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-191">c.</span></span> <span data-ttu-id="f5a0f-192">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f5a0f-193">d.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-193">d.</span></span> <span data-ttu-id="f5a0f-194">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-194">Click **Create**.</span></span>
 
### <a name="creating-a-jobbadmin-test-user"></a><span data-ttu-id="f5a0f-195">Vytvoření zkušebního uživatele Jobbadmin</span><span class="sxs-lookup"><span data-stu-id="f5a0f-195">Creating a Jobbadmin test user</span></span>

<span data-ttu-id="f5a0f-196">Pokud chcete povolit uživatelům Azure AD přihlášení k Jobbadmin, musí být zřízená do Jobbadmin.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-196">To enable Azure AD users to log in to Jobbadmin, they must be provisioned into Jobbadmin.</span></span>
 
<span data-ttu-id="f5a0f-197">Obraťte se na [tým podpory Jobbadmin](https://www.jobbnorge.no/om-oss/kontakt-oss) získat uživatele, přidání na jejich straně.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-197">Please contact [Jobbadmin support team](https://www.jobbnorge.no/om-oss/kontakt-oss) to get the users added on their side.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f5a0f-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f5a0f-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f5a0f-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Jobbadmin.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jobbadmin.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f5a0f-201">**Pokud chcete přiřadit Britta Simon Jobbadmin, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f5a0f-201">**To assign Britta Simon to Jobbadmin, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a0f-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f5a0f-204">V seznamu aplikací vyberte **Jobbadmin**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-204">In the applications list, select **Jobbadmin**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-jobbadmin-tutorial/tutorial_jobbadmin_app.png) 

3. <span data-ttu-id="f5a0f-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f5a0f-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-208">Click **Add** button.</span></span> <span data-ttu-id="f5a0f-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f5a0f-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f5a0f-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f5a0f-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f5a0f-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f5a0f-214">Testing single sign-on</span></span>

<span data-ttu-id="f5a0f-215">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f5a0f-216">Když kliknete na dlaždici Jobbadmin na přístupovém panelu, měli byste obdržet přihlašovací stránku Jobbadmin aplikace.</span><span class="sxs-lookup"><span data-stu-id="f5a0f-216">When you click the Jobbadmin tile in the Access Panel, you should get login page of Jobbadmin application.</span></span>
<span data-ttu-id="f5a0f-217">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f5a0f-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f5a0f-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f5a0f-218">Additional resources</span></span>

* [<span data-ttu-id="f5a0f-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f5a0f-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f5a0f-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f5a0f-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobbadmin-tutorial/tutorial_general_203.png

