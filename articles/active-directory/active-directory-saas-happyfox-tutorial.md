---
title: 'Kurz: Azure Active Directory integrace s HappyFox | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a HappyFox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 8b5ad750d7849e4037ed7ee93c48b5645c68e799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a><span data-ttu-id="3eb02-103">Kurz: Azure Active Directory integrace s HappyFox</span><span class="sxs-lookup"><span data-stu-id="3eb02-103">Tutorial: Azure Active Directory integration with HappyFox</span></span>

<span data-ttu-id="3eb02-104">V tomto kurzu zjistěte, jak integrovat HappyFox s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3eb02-104">In this tutorial, you learn how to integrate HappyFox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3eb02-105">Integrace HappyFox s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="3eb02-105">Integrating HappyFox with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3eb02-106">Můžete řídit ve službě Azure AD, který má přístup k HappyFox</span><span class="sxs-lookup"><span data-stu-id="3eb02-106">You can control in Azure AD who has access to HappyFox</span></span>
- <span data-ttu-id="3eb02-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k HappyFox (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb02-107">You can enable your users to automatically get signed-on to HappyFox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3eb02-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3eb02-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3eb02-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3eb02-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3eb02-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3eb02-110">Prerequisites</span></span>

<span data-ttu-id="3eb02-111">Konfigurace integrace Azure AD s HappyFox, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="3eb02-111">To configure Azure AD integration with HappyFox, you need the following items:</span></span>

- <span data-ttu-id="3eb02-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb02-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3eb02-113">HappyFox jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="3eb02-113">A HappyFox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3eb02-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="3eb02-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3eb02-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="3eb02-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3eb02-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="3eb02-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3eb02-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3eb02-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3eb02-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="3eb02-118">Scenario description</span></span>
<span data-ttu-id="3eb02-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="3eb02-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3eb02-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="3eb02-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3eb02-121">Přidání HappyFox z Galerie</span><span class="sxs-lookup"><span data-stu-id="3eb02-121">Adding HappyFox from the gallery</span></span>
2. <span data-ttu-id="3eb02-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb02-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-happyfox-from-the-gallery"></a><span data-ttu-id="3eb02-123">Přidání HappyFox z Galerie</span><span class="sxs-lookup"><span data-stu-id="3eb02-123">Adding HappyFox from the gallery</span></span>
<span data-ttu-id="3eb02-124">Při konfiguraci integrace HappyFox do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS HappyFox z galerie.</span><span class="sxs-lookup"><span data-stu-id="3eb02-124">To configure the integration of HappyFox into Azure AD, you need to add HappyFox from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3eb02-125">**Pokud chcete přidat HappyFox z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3eb02-125">**To add HappyFox from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3eb02-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3eb02-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3eb02-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3eb02-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="3eb02-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb02-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="3eb02-133">Do vyhledávacího pole zadejte **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-133">In the search box, type **HappyFox**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. <span data-ttu-id="3eb02-135">Na panelu výsledků vyberte **HappyFox**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3eb02-135">In the results panel, select **HappyFox**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3eb02-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb02-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3eb02-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HappyFox podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="3eb02-138">In this section, you configure and test Azure AD single sign-on with HappyFox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3eb02-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v HappyFox je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3eb02-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HappyFox is to a user in Azure AD.</span></span> <span data-ttu-id="3eb02-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v HappyFox musí navázat.</span><span class="sxs-lookup"><span data-stu-id="3eb02-140">In other words, a link relationship between an Azure AD user and the related user in HappyFox needs to be established.</span></span>

<span data-ttu-id="3eb02-141">V HappyFox, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="3eb02-141">In HappyFox, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3eb02-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s HappyFox, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="3eb02-142">To configure and test Azure AD single sign-on with HappyFox, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3eb02-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="3eb02-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3eb02-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3eb02-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3eb02-145">**[Vytvoření zkušebního uživatele HappyFox](#creating-a-happyfox-test-user)**  – Pokud chcete mít protějšek Britta Simon v HappyFox propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="3eb02-145">**[Creating a HappyFox test user](#creating-a-happyfox-test-user)** - to have a counterpart of Britta Simon in HappyFox that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3eb02-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3eb02-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3eb02-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3eb02-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3eb02-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb02-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3eb02-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci HappyFox.</span><span class="sxs-lookup"><span data-stu-id="3eb02-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HappyFox application.</span></span>

<span data-ttu-id="3eb02-150">**Ke konfiguraci Azure AD jednotné přihlašování s HappyFox, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3eb02-150">**To configure Azure AD single sign-on with HappyFox, perform the following steps:**</span></span>

1. <span data-ttu-id="3eb02-151">Na portálu Azure na **HappyFox** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-151">In the Azure portal, on the **HappyFox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="3eb02-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="3eb02-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. <span data-ttu-id="3eb02-155">Na **HappyFox domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3eb02-155">On the **HappyFox Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    <span data-ttu-id="3eb02-157">a.</span><span class="sxs-lookup"><span data-stu-id="3eb02-157">a.</span></span> <span data-ttu-id="3eb02-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.happyfox.com/`</span><span class="sxs-lookup"><span data-stu-id="3eb02-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.happyfox.com/`</span></span>

    <span data-ttu-id="3eb02-159">b.</span><span class="sxs-lookup"><span data-stu-id="3eb02-159">b.</span></span> <span data-ttu-id="3eb02-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.happyfox.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="3eb02-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.happyfox.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3eb02-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="3eb02-161">These values are not real.</span></span> <span data-ttu-id="3eb02-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="3eb02-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3eb02-163">Obraťte se na [tým podpory HappyFox klienta](https://support.happyfox.com/home) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="3eb02-163">Contact [HappyFox Client support team](https://support.happyfox.com/home) to get these values.</span></span> 
 
4. <span data-ttu-id="3eb02-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="3eb02-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. <span data-ttu-id="3eb02-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3eb02-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3eb02-168">Na **HappyFox konfigurace** klikněte na tlačítko **konfigurace HappyFox** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="3eb02-168">On the **HappyFox Configuration** section, click **Configure HappyFox** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3eb02-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. <span data-ttu-id="3eb02-171">Přihlaste se k portálu HappyFox zaměstnanci a přejděte do **spravovat**, klikněte na **integrace** kartě.</span><span class="sxs-lookup"><span data-stu-id="3eb02-171">Sign on to your HappyFox staff portal and navigate to **Manage**, Click on **Integrations** tab.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. <span data-ttu-id="3eb02-173">Na kartě Integrace klikněte na tlačítko **konfigurace** pod **SAML integrace** otevřete jeden znak na nastavení.</span><span class="sxs-lookup"><span data-stu-id="3eb02-173">In the Integrations tab, Click **Configure** under **SAML Integration** to open the Single Sign On Settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. <span data-ttu-id="3eb02-175">V části Konfigurace SAML, vložte **SAML jeden přihlašování adresa URL služby** kterou jste zkopírovali z portálu Azure do **jednotné přihlašování cílová adresa URL** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3eb02-175">Inside SAML configuration section, paste the **SAML Single Sign-On Service URL** that you have copied from Azure portal into **SSO Target URL** textbox.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. <span data-ttu-id="3eb02-177">Otevřete certifikát stažený z portálu Azure v programu Poznámkový blok a vložte jeho obsah v **IdP podpis** části.</span><span class="sxs-lookup"><span data-stu-id="3eb02-177">Open the certificate downloaded from Azure portal in notepad and paste its content in **IdP Signature** section.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. <span data-ttu-id="3eb02-179">Klikněte na tlačítko **uložit nastavení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3eb02-179">Click **Save Settings** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> <span data-ttu-id="3eb02-181">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="3eb02-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3eb02-182">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="3eb02-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3eb02-183">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3eb02-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3eb02-184">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb02-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="3eb02-185">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3eb02-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="3eb02-187">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3eb02-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3eb02-188">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="3eb02-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3eb02-190">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3eb02-192">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb02-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3eb02-194">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3eb02-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3eb02-196">a.</span><span class="sxs-lookup"><span data-stu-id="3eb02-196">a.</span></span> <span data-ttu-id="3eb02-197">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3eb02-198">b.</span><span class="sxs-lookup"><span data-stu-id="3eb02-198">b.</span></span> <span data-ttu-id="3eb02-199">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3eb02-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3eb02-200">c.</span><span class="sxs-lookup"><span data-stu-id="3eb02-200">c.</span></span> <span data-ttu-id="3eb02-201">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3eb02-202">d.</span><span class="sxs-lookup"><span data-stu-id="3eb02-202">d.</span></span> <span data-ttu-id="3eb02-203">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-203">Click **Create**.</span></span>
 
### <a name="creating-a-happyfox-test-user"></a><span data-ttu-id="3eb02-204">Vytvoření zkušebního uživatele HappyFox</span><span class="sxs-lookup"><span data-stu-id="3eb02-204">Creating a HappyFox test user</span></span>

<span data-ttu-id="3eb02-205">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3eb02-205">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3eb02-206">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="3eb02-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3eb02-207">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu HappyFox.</span><span class="sxs-lookup"><span data-stu-id="3eb02-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HappyFox.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="3eb02-209">**Pokud chcete přiřadit Britta Simon HappyFox, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="3eb02-209">**To assign Britta Simon to HappyFox, perform the following steps:**</span></span>

1. <span data-ttu-id="3eb02-210">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="3eb02-212">V seznamu aplikací vyberte **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-212">In the applications list, select **HappyFox**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. <span data-ttu-id="3eb02-214">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="3eb02-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="3eb02-216">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="3eb02-216">Click **Add** button.</span></span> <span data-ttu-id="3eb02-217">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb02-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="3eb02-219">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="3eb02-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3eb02-220">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb02-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3eb02-221">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3eb02-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3eb02-222">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="3eb02-222">Testing single sign-on</span></span>

<span data-ttu-id="3eb02-223">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="3eb02-223">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="3eb02-224">Když kliknete na dlaždici HappyFox na přístupovém panelu, měli byste obdržet přihlašovací stránku HappyFox aplikace.</span><span class="sxs-lookup"><span data-stu-id="3eb02-224">When you click the HappyFox tile in the Access Panel, you should get login page of HappyFox application.</span></span> <span data-ttu-id="3eb02-225">Měli byste vidět **'SAML'** tlačítko na přihlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="3eb02-225">You should see the **‘SAML’** button on the sign-in page.</span></span>

    ![Modul plug-in](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. <span data-ttu-id="3eb02-227">Klikněte **'SAML,** tlačítko pro přihlášení do HappyFox pomocí účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3eb02-227">Click the **‘SAML’** button to log in to HappyFox using your Azure AD account.</span></span>

<span data-ttu-id="3eb02-228">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3eb02-228">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3eb02-229">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3eb02-229">Additional resources</span></span>

* [<span data-ttu-id="3eb02-230">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3eb02-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3eb02-231">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3eb02-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

