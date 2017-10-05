---
title: "Kurz: Azure Active Directory integrace s KnowBe4 zabezpečení sledování školení | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a KnowBe4 zabezpečení sledování školení."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b80d2212-cc5f-4adb-836c-570640810c39
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 3b18737112a8aef101fab7fac1904f7c2e194d64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-knowbe4-security-awareness-training"></a><span data-ttu-id="dbb72-103">Kurz: Azure Active Directory integrace s KnowBe4 zabezpečení sledování školení</span><span class="sxs-lookup"><span data-stu-id="dbb72-103">Tutorial: Azure Active Directory integration with KnowBe4 Security Awareness Training</span></span>

<span data-ttu-id="dbb72-104">V tomto kurzu zjistěte, jak integrovat KnowBe4 zabezpečení sledování školení s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dbb72-104">In this tutorial, you learn how to integrate KnowBe4 Security Awareness Training with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dbb72-105">Integrace KnowBe4 zabezpečení sledování školení s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="dbb72-105">Integrating KnowBe4 Security Awareness Training with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dbb72-106">Můžete řídit ve službě Azure AD, který má přístup k KnowBe4 zabezpečení sledování školení</span><span class="sxs-lookup"><span data-stu-id="dbb72-106">You can control in Azure AD who has access to KnowBe4 Security Awareness Training</span></span>
- <span data-ttu-id="dbb72-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k KnowBe4 bezpečnostního školení sledování (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="dbb72-107">You can enable your users to automatically get signed-on to KnowBe4 Security Awareness Training (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dbb72-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="dbb72-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dbb72-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dbb72-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dbb72-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dbb72-110">Prerequisites</span></span>

<span data-ttu-id="dbb72-111">Ke konfiguraci integrace služby Azure AD se školením sledování KnowBe4 zabezpečení, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="dbb72-111">To configure Azure AD integration with KnowBe4 Security Awareness Training, you need the following items:</span></span>

- <span data-ttu-id="dbb72-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="dbb72-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dbb72-113">Školení sledování zabezpečení KnowBe4 jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="dbb72-113">A KnowBe4 Security Awareness Training single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dbb72-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="dbb72-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dbb72-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="dbb72-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dbb72-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="dbb72-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dbb72-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dbb72-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dbb72-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="dbb72-118">Scenario description</span></span>
<span data-ttu-id="dbb72-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="dbb72-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dbb72-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="dbb72-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dbb72-121">Přidání KnowBe4 zabezpečení sledování školení z Galerie</span><span class="sxs-lookup"><span data-stu-id="dbb72-121">Adding KnowBe4 Security Awareness Training from the gallery</span></span>
2. <span data-ttu-id="dbb72-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dbb72-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-knowbe4-security-awareness-training-from-the-gallery"></a><span data-ttu-id="dbb72-123">Přidání KnowBe4 zabezpečení sledování školení z Galerie</span><span class="sxs-lookup"><span data-stu-id="dbb72-123">Adding KnowBe4 Security Awareness Training from the gallery</span></span>
<span data-ttu-id="dbb72-124">Při konfiguraci integrace KnowBe4 zabezpečení sledování školení do služby Azure AD, potřebujete přidat školení sledování zabezpečení KnowBe4 z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="dbb72-124">To configure the integration of KnowBe4 Security Awareness Training into Azure AD, you need to add KnowBe4 Security Awareness Training from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dbb72-125">**Chcete-li přidat KnowBe4 zabezpečení sledování školení z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dbb72-125">**To add KnowBe4 Security Awareness Training from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dbb72-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dbb72-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dbb72-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dbb72-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="dbb72-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dbb72-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="dbb72-133">Do vyhledávacího pole zadejte **KnowBe4 zabezpečení sledování školení**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-133">In the search box, type **KnowBe4 Security Awareness Training**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_search.png)

5. <span data-ttu-id="dbb72-135">Na panelu výsledků vyberte **KnowBe4 zabezpečení sledování školení**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dbb72-135">In the results panel, select **KnowBe4 Security Awareness Training**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dbb72-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="dbb72-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dbb72-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s KnowBe4 zabezpečení sledování školení podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="dbb72-138">In this section, you configure and test Azure AD single sign-on with KnowBe4 Security Awareness Training based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dbb72-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v školení sledování KnowBe4 zabezpečení je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dbb72-139">For single sign-on to work, Azure AD needs to know what the counterpart user in KnowBe4 Security Awareness Training is to a user in Azure AD.</span></span> <span data-ttu-id="dbb72-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v KnowBe4 zabezpečení sledování školení musí navázat.</span><span class="sxs-lookup"><span data-stu-id="dbb72-140">In other words, a link relationship between an Azure AD user and the related user in KnowBe4 Security Awareness Training needs to be established.</span></span>

<span data-ttu-id="dbb72-141">V KnowBe4 zabezpečení sledování školení, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="dbb72-141">In KnowBe4 Security Awareness Training, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dbb72-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování se školením sledování KnowBe4 zabezpečení, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="dbb72-142">To configure and test Azure AD single sign-on with KnowBe4 Security Awareness Training, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dbb72-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="dbb72-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dbb72-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dbb72-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dbb72-145">**[Vytvoření zkušebního uživatele školení sledování zabezpečení KnowBe4](#creating-a-knowbe4-security-awareness-training-test-user)**  – Pokud chcete mít protějšek Britta Simon v KnowBe4 zabezpečení sledování školení propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="dbb72-145">**[Creating a KnowBe4 Security Awareness Training test user](#creating-a-knowbe4-security-awareness-training-test-user)** - to have a counterpart of Britta Simon in KnowBe4 Security Awareness Training that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dbb72-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dbb72-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dbb72-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="dbb72-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dbb72-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dbb72-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dbb72-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci KnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="dbb72-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your KnowBe4 Security Awareness Training application.</span></span>

<span data-ttu-id="dbb72-150">**Ke konfiguraci Azure AD jednotné přihlašování se školením sledování zabezpečení KnowBe4, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dbb72-150">**To configure Azure AD single sign-on with KnowBe4 Security Awareness Training, perform the following steps:**</span></span>

1. <span data-ttu-id="dbb72-151">Na portálu Azure na **KnowBe4 zabezpečení sledování školení** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-151">In the Azure portal, on the **KnowBe4 Security Awareness Training** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="dbb72-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dbb72-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_samlbase.png)

3. <span data-ttu-id="dbb72-155">Na **KnowBe4 zabezpečení sledování školení domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dbb72-155">On the **KnowBe4 Security Awareness Training Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_url.png)

    <span data-ttu-id="dbb72-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<companyname>.KnowBe4.com/auth/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="dbb72-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.KnowBe4.com/auth/saml/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dbb72-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="dbb72-158">The value is not real.</span></span> <span data-ttu-id="dbb72-159">Aktualizujte hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="dbb72-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="dbb72-160">Obraťte se na [tým podpory klienta školení sledování zabezpečení KnowBe4](mailto:support@KnowBe4.com) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="dbb72-160">Contact [KnowBe4 Security Awareness Training Client support team](mailto:support@KnowBe4.com) to get the value.</span></span> 
 

4. <span data-ttu-id="dbb72-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Raw)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="dbb72-161">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_certificate.png) 

5. <span data-ttu-id="dbb72-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dbb72-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dbb72-165">Na **konfigurace školení sledování zabezpečení KnowBe4** klikněte na tlačítko **konfigurace KnowBe4 zabezpečení sledování školení** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="dbb72-165">On the **KnowBe4 Security Awareness Training Configuration** section, click **Configure KnowBe4 Security Awareness Training** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dbb72-166">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="dbb72-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_configure.png) 

7. <span data-ttu-id="dbb72-168">Konfigurace jednotného přihlašování na **KnowBe4 zabezpečení sledování školení** straně, budete muset odeslat stažené **certifikátu (Raw)**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory KnowBe4 zabezpečení sledování školení klienta](mailto:support@KnowBe4.com).</span><span class="sxs-lookup"><span data-stu-id="dbb72-168">To configure single sign-on on **KnowBe4 Security Awareness Training** side, you need to send the downloaded **Certificate (Raw)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [KnowBe4 Security Awareness Training Client support team](mailto:support@KnowBe4.com).</span></span>

> [!TIP]
> <span data-ttu-id="dbb72-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="dbb72-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dbb72-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="dbb72-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dbb72-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dbb72-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dbb72-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dbb72-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="dbb72-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dbb72-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="dbb72-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dbb72-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dbb72-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="dbb72-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dbb72-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dbb72-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dbb72-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dbb72-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="dbb72-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-KnowBe4-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dbb72-184">a.</span><span class="sxs-lookup"><span data-stu-id="dbb72-184">a.</span></span> <span data-ttu-id="dbb72-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dbb72-186">b.</span><span class="sxs-lookup"><span data-stu-id="dbb72-186">b.</span></span> <span data-ttu-id="dbb72-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dbb72-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dbb72-188">c.</span><span class="sxs-lookup"><span data-stu-id="dbb72-188">c.</span></span> <span data-ttu-id="dbb72-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dbb72-190">d.</span><span class="sxs-lookup"><span data-stu-id="dbb72-190">d.</span></span> <span data-ttu-id="dbb72-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-191">Click **Create**.</span></span>
 
### <a name="creating-a-knowbe4-security-awareness-training-test-user"></a><span data-ttu-id="dbb72-192">Vytvoření zkušebního uživatele KnowBe4 zabezpečení sledování školení</span><span class="sxs-lookup"><span data-stu-id="dbb72-192">Creating a KnowBe4 Security Awareness Training test user</span></span>

<span data-ttu-id="dbb72-193">Cílem této části je vytvoření uživatele volal Britta Simon v KnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="dbb72-193">The objective of this section is to create a user called Britta Simon in KnowBe4 Security Awareness Training.</span></span> <span data-ttu-id="dbb72-194">Školení sledování zabezpečení KnowBe4 podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="dbb72-194">KnowBe4 Security Awareness Training supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="dbb72-195">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="dbb72-195">There is no action item for you in this section.</span></span> <span data-ttu-id="dbb72-196">Nový uživatel se vytvoří během pokusu o přístup k KnowBe4 zabezpečení sledování školení, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="dbb72-196">A new user is created during an attempt to access KnowBe4 Security Awareness Training if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="dbb72-197">Pokud potřebujete ručně vytvořit uživatele, budete muset kontaktovat [tým podpory školení sledování zabezpečení KnowBe4](mailto:support@KnowBe4.com).</span><span class="sxs-lookup"><span data-stu-id="dbb72-197">If you need to create a user manually, you need to contact the [KnowBe4 Security Awareness Training support team](mailto:support@KnowBe4.com).</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dbb72-198">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="dbb72-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dbb72-199">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu KnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="dbb72-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to KnowBe4 Security Awareness Training.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="dbb72-201">**Pokud chcete přiřadit Britta Simon KnowBe4 zabezpečení sledování školení, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="dbb72-201">**To assign Britta Simon to KnowBe4 Security Awareness Training, perform the following steps:**</span></span>

1. <span data-ttu-id="dbb72-202">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="dbb72-204">V seznamu aplikací vyberte **KnowBe4 zabezpečení sledování školení**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-204">In the applications list, select **KnowBe4 Security Awareness Training**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-KnowBe4-tutorial/tutorial_knowbe4sat_app.png) 

3. <span data-ttu-id="dbb72-206">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="dbb72-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="dbb72-208">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="dbb72-208">Click **Add** button.</span></span> <span data-ttu-id="dbb72-209">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dbb72-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="dbb72-211">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="dbb72-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dbb72-212">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dbb72-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dbb72-213">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="dbb72-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dbb72-214">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="dbb72-214">Testing single sign-on</span></span>

<span data-ttu-id="dbb72-215">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="dbb72-215">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>
  
<span data-ttu-id="dbb72-216">Když kliknete na dlaždici KnowBe4 zabezpečení sledování školení na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci KnowBe4 zabezpečení sledování školení.</span><span class="sxs-lookup"><span data-stu-id="dbb72-216">When you click the KnowBe4 Security Awareness Training tile in the Access Panel, you should get automatically signed-on to your KnowBe4 Security Awareness Training application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dbb72-217">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="dbb72-217">Additional resources</span></span>

* [<span data-ttu-id="dbb72-218">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dbb72-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dbb72-219">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dbb72-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-KnowBe4-tutorial/tutorial_general_203.png

