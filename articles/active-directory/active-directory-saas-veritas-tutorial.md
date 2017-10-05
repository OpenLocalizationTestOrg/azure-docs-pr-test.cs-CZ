---
title: "Kurz: Integrace Azure Active Directory této společnosti Enterprise Vault.cloud jednotného přihlašování k | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování Vault.cloud Enterprise této společnosti."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 1f8c7fd97021f320a23bc78466a7b61f2d7e513e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a><span data-ttu-id="a4179-103">Kurz: Integrace Azure Active Directory pomocí této společnosti Enterprise Vault.cloud jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-103">Tutorial: Azure Active Directory integration with Veritas Enterprise Vault.cloud SSO</span></span>

<span data-ttu-id="a4179-104">V tomto kurzu zjistěte, jak k této společnosti Enterprise Vault.cloud jednotného přihlašování k integraci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a4179-104">In this tutorial, you learn how to integrate Veritas Enterprise Vault.cloud SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a4179-105">Integrace této společnosti Enterprise Vault.cloud jednotné přihlašování s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a4179-105">Integrating Veritas Enterprise Vault.cloud SSO with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a4179-106">Můžete řídit ve službě Azure AD, který má přístup k této společnosti Enterprise Vault.cloud jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-106">You can control in Azure AD who has access to Veritas Enterprise Vault.cloud SSO</span></span>
- <span data-ttu-id="a4179-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k této společnosti Enterprise Vault.cloud jednotné přihlašování (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4179-107">You can enable your users to automatically get signed-on to Veritas Enterprise Vault.cloud SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a4179-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a4179-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a4179-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a4179-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4179-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a4179-110">Prerequisites</span></span>

<span data-ttu-id="a4179-111">Ke konfiguraci integrace služby Azure AD pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a4179-111">To configure Azure AD integration with Veritas Enterprise Vault.cloud SSO, you need the following items:</span></span>

- <span data-ttu-id="a4179-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4179-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a4179-113">Této společnosti Enterprise Vault.cloud jednotného přihlašování k jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a4179-113">A Veritas Enterprise Vault.cloud SSO single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a4179-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4179-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a4179-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a4179-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a4179-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a4179-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a4179-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a4179-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a4179-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a4179-118">Scenario description</span></span>
<span data-ttu-id="a4179-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a4179-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a4179-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a4179-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a4179-121">Přidání této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie</span><span class="sxs-lookup"><span data-stu-id="a4179-121">Adding Veritas Enterprise Vault.cloud SSO from the gallery</span></span>
2. <span data-ttu-id="a4179-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a><span data-ttu-id="a4179-123">Přidání této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie</span><span class="sxs-lookup"><span data-stu-id="a4179-123">Adding Veritas Enterprise Vault.cloud SSO from the gallery</span></span>
<span data-ttu-id="a4179-124">Při konfiguraci integrace této společnosti Enterprise Vault.cloud přihlašování do služby Azure AD, potřebujete přidat této společnosti Enterprise Vault.cloud jednotného přihlašování z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a4179-124">To configure the integration of Veritas Enterprise Vault.cloud SSO into Azure AD, you need to add Veritas Enterprise Vault.cloud SSO from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a4179-125">**Pokud chcete přidat této společnosti Enterprise Vault.cloud jednotného přihlašování z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="a4179-125">**To add Veritas Enterprise Vault.cloud SSO from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a4179-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4179-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a4179-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a4179-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a4179-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4179-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a4179-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4179-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a4179-133">Do vyhledávacího pole zadejte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="a4179-133">In the search box, type **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. <span data-ttu-id="a4179-135">Na panelu výsledků vyberte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4179-135">In the results panel, select **Veritas Enterprise Vault.cloud SSO**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a4179-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a4179-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí této společnosti Enterprise Vault.cloud jednotného přihlašování na základě testovací uživatele, nazývá "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a4179-138">In this section, you configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a4179-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co příslušného uživatele v této společnosti Enterprise Vault.cloud jednotné přihlašování je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a4179-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Veritas Enterprise Vault.cloud SSO is to a user in Azure AD.</span></span> <span data-ttu-id="a4179-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v této společnosti Enterprise Vault.cloud jednotné přihlašování musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a4179-140">In other words, a link relationship between an Azure AD user and the related user in Veritas Enterprise Vault.cloud SSO needs to be established.</span></span>

<span data-ttu-id="a4179-141">V této společnosti Enterprise Vault.cloud jednotné přihlašování, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a4179-141">In Veritas Enterprise Vault.cloud SSO, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a4179-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a4179-142">To configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a4179-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a4179-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a4179-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4179-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a4179-145">**[Vytvoření zkušebního uživatele této společnosti Enterprise Vault.cloud jednotného přihlašování k](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  – Pokud chcete mít protějšek Britta Simon v této společnosti Enterprise Vault.cloud jednotného přihlašování k propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a4179-145">**[Creating a Veritas Enterprise Vault.cloud SSO test user](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)** - to have a counterpart of Britta Simon in Veritas Enterprise Vault.cloud SSO that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a4179-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4179-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a4179-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a4179-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a4179-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a4179-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v této společnosti Enterprise Vault.cloud jednotného přihlašování k aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a4179-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Veritas Enterprise Vault.cloud SSO application.</span></span>

<span data-ttu-id="a4179-150">**Ke konfiguraci Azure AD jednotné přihlašování pomocí jednotného přihlašování Vault.cloud Enterprise této společnosti, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4179-150">**To configure Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="a4179-151">Na portálu Azure na **této společnosti Enterprise Vault.cloud jednotného přihlašování k** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a4179-151">In the Azure portal, on the **Veritas Enterprise Vault.cloud SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a4179-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4179-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. <span data-ttu-id="a4179-155">Na **této společnosti Enterprise Vault.cloud jednotného přihlašování k doméně a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4179-155">On the **Veritas Enterprise Vault.cloud SSO Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    <span data-ttu-id="a4179-157">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span><span class="sxs-lookup"><span data-stu-id="a4179-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="a4179-158">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="a4179-158">This value is not real.</span></span> <span data-ttu-id="a4179-159">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4179-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="a4179-160">Obraťte se na [tým podpory této společnosti Enterprise Vault.cloud jednotné přihlašování klienta](https://www.veritas.com/support/.html) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a4179-160">Contact [Veritas Enterprise Vault.cloud SSO Client support team](https://www.veritas.com/support/.html) to get this value.</span></span> 

4. <span data-ttu-id="a4179-161">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a4179-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. <span data-ttu-id="a4179-163">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4179-163">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a4179-165">Na **konfiguraci této společnosti Enterprise Vault.cloud jednotného přihlašování k** klikněte na tlačítko **konfigurace této společnosti Enterprise Vault.cloud SSO** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a4179-165">On the **Veritas Enterprise Vault.cloud SSO Configuration** section, click **Configure Veritas Enterprise Vault.cloud SSO** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a4179-166">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a4179-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. <span data-ttu-id="a4179-168">Konfigurace jednotného přihlašování na **této společnosti Enterprise Vault.cloud jednotného přihlašování k** straně, budete muset odeslat stažené **Certificate(Base64)** a **SAML jeden přihlašování adresa URL služby** k [tým podpory této společnosti Enterprise Vault.cloud jednotného přihlašování k](https://www.veritas.com/support/.html).</span><span class="sxs-lookup"><span data-stu-id="a4179-168">To configure single sign-on on **Veritas Enterprise Vault.cloud SSO** side, you need to send the downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** to [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html).</span></span>

> [!TIP]
> <span data-ttu-id="a4179-169">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a4179-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a4179-170">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a4179-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a4179-171">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a4179-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a4179-172">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4179-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="a4179-173">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4179-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a4179-175">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4179-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a4179-176">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a4179-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a4179-178">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a4179-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a4179-180">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4179-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a4179-182">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a4179-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a4179-184">a.</span><span class="sxs-lookup"><span data-stu-id="a4179-184">a.</span></span> <span data-ttu-id="a4179-185">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4179-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a4179-186">b.</span><span class="sxs-lookup"><span data-stu-id="a4179-186">b.</span></span> <span data-ttu-id="a4179-187">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a4179-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a4179-188">c.</span><span class="sxs-lookup"><span data-stu-id="a4179-188">c.</span></span> <span data-ttu-id="a4179-189">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a4179-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a4179-190">d.</span><span class="sxs-lookup"><span data-stu-id="a4179-190">d.</span></span> <span data-ttu-id="a4179-191">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a4179-191">Click **Create**.</span></span>
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a><span data-ttu-id="a4179-192">Vytvoření zkušebního uživatele této společnosti Enterprise Vault.cloud jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-192">Creating a Veritas Enterprise Vault.cloud SSO test user</span></span>

<span data-ttu-id="a4179-193">V této části vytvoříte uživatele volat Britta Simon SSO Vault.cloud Enterprise.</span><span class="sxs-lookup"><span data-stu-id="a4179-193">In this section, you create a user called Britta Simon in Enterprise Vault.cloud SSO.</span></span> <span data-ttu-id="a4179-194">Práce s [tým podpory této společnosti Enterprise Vault.cloud jednotného přihlašování k](https://www.veritas.com/support/.html) přidejte uživatele v podniku Vault.cloud jednotného přihlašování k platformě.</span><span class="sxs-lookup"><span data-stu-id="a4179-194">Work with [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html) to add the users in the Enterprise Vault.cloud SSO platform.</span></span> <span data-ttu-id="a4179-195">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4179-195">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a4179-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a4179-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a4179-197">V této části povolíte Britta Simon k používání Azure jednotné přihlašování v této společnosti Enterprise Vault.cloud jednotného přihlašování k udělení přístupu.</span><span class="sxs-lookup"><span data-stu-id="a4179-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Veritas Enterprise Vault.cloud SSO.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a4179-199">**Pokud chcete přiřadit Britta Simon této společnosti Enterprise Vault.cloud jednotné přihlašování, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a4179-199">**To assign Britta Simon to Veritas Enterprise Vault.cloud SSO, perform the following steps:**</span></span>

1. <span data-ttu-id="a4179-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a4179-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a4179-202">V seznamu aplikací vyberte **této společnosti Enterprise Vault.cloud jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="a4179-202">In the applications list, select **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. <span data-ttu-id="a4179-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a4179-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a4179-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a4179-206">Click **Add** button.</span></span> <span data-ttu-id="a4179-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4179-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a4179-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a4179-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a4179-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4179-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a4179-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a4179-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a4179-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a4179-212">Testing single sign-on</span></span>

<span data-ttu-id="a4179-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a4179-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a4179-214">Když kliknete na dlaždici této společnosti Enterprise Vault.cloud jednotného přihlašování na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci této společnosti Enterprise Vault.cloud jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a4179-214">When you click the Veritas Enterprise Vault.cloud SSO tile in the Access Panel, you should get automatically signed-on to your Veritas Enterprise Vault.cloud SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4179-215">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a4179-215">Additional resources</span></span>

* [<span data-ttu-id="a4179-216">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a4179-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a4179-217">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a4179-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

