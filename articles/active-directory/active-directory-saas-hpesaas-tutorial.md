---
title: 'Kurz: Azure Active Directory integrace s HPE SaaS | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a HPE SaaS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 314003d6-ca66-4456-88c3-934254d4a9a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: 5e6f0da531df85359aa47477248dd020a039e7e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hpe-saas"></a><span data-ttu-id="2f2a0-103">Kurz: Azure Active Directory integrace s HPE SaaS</span><span class="sxs-lookup"><span data-stu-id="2f2a0-103">Tutorial: Azure Active Directory integration with HPE SaaS</span></span>

<span data-ttu-id="2f2a0-104">V tomto kurzu zjistěte, jak integrovat HPE SaaS s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2f2a0-104">In this tutorial, you learn how to integrate HPE SaaS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2f2a0-105">Integrace s Azure AD HPE SaaS nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-105">Integrating HPE SaaS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2f2a0-106">Můžete řídit ve službě Azure AD, který má přístup k HPE SaaS</span><span class="sxs-lookup"><span data-stu-id="2f2a0-106">You can control in Azure AD who has access to HPE SaaS</span></span>
- <span data-ttu-id="2f2a0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k HPE SaaS (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f2a0-107">You can enable your users to automatically get signed-on to HPE SaaS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2f2a0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2f2a0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2f2a0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2f2a0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f2a0-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2f2a0-110">Prerequisites</span></span>

<span data-ttu-id="2f2a0-111">Konfigurace integrace Azure AD s HPE SaaS, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-111">To configure Azure AD integration with HPE SaaS, you need the following items:</span></span>

- <span data-ttu-id="2f2a0-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f2a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2f2a0-113">HPE SaaS jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2f2a0-113">An HPE SaaS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2f2a0-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2f2a0-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2f2a0-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2f2a0-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební: [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2f2a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2f2a0-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2f2a0-118">Scenario description</span></span>
<span data-ttu-id="2f2a0-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2f2a0-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2f2a0-121">Přidání HPE SaaS z Galerie</span><span class="sxs-lookup"><span data-stu-id="2f2a0-121">Adding HPE SaaS from the gallery</span></span>
2. <span data-ttu-id="2f2a0-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f2a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hpe-saas-from-the-gallery"></a><span data-ttu-id="2f2a0-123">Přidání HPE SaaS z Galerie</span><span class="sxs-lookup"><span data-stu-id="2f2a0-123">Adding HPE SaaS from the gallery</span></span>
<span data-ttu-id="2f2a0-124">Při konfiguraci integrace HPE SaaS do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS HPE SaaS z galerie.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-124">To configure the integration of HPE SaaS into Azure AD, you need to add HPE SaaS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2f2a0-125">**Pokud chcete přidat HPE SaaS z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f2a0-125">**To add HPE SaaS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2f2a0-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2f2a0-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2f2a0-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2f2a0-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2f2a0-133">Do vyhledávacího pole zadejte **HPE SaaS**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-133">In the search box, type **HPE SaaS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_search.png)

5. <span data-ttu-id="2f2a0-135">Na panelu výsledků vyberte **HPE SaaS**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-135">In the results panel, select **HPE SaaS**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2f2a0-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f2a0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2f2a0-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s HPE SaaS podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="2f2a0-138">In this section, you configure and test Azure AD single sign-on with HPE SaaS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2f2a0-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v HPE SaaS je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HPE SaaS is to a user in Azure AD.</span></span> <span data-ttu-id="2f2a0-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v HPE SaaS musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-140">In other words, a link relationship between an Azure AD user and the related user in HPE SaaS needs to be established.</span></span>

<span data-ttu-id="2f2a0-141">V HPE SaaS přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-141">In HPE SaaS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2f2a0-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s HPE SaaS, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-142">To configure and test Azure AD single sign-on with HPE SaaS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2f2a0-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2f2a0-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2f2a0-145">**[Vytváření testovacího uživatele HPE SaaS](#creating-an-hpe-saas-test-user)**  – Pokud chcete mít protějšek Britta Simon v HPE SaaS, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-145">**[Creating an HPE SaaS test user](#creating-an-hpe-saas-test-user)** - to have a counterpart of Britta Simon in HPE SaaS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2f2a0-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2f2a0-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2f2a0-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f2a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2f2a0-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci HPE SaaS.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HPE SaaS application.</span></span>

<span data-ttu-id="2f2a0-150">**Ke konfiguraci Azure AD jednotné přihlašování s HPE SaaS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f2a0-150">**To configure Azure AD single sign-on with HPE SaaS, perform the following steps:**</span></span>

1. <span data-ttu-id="2f2a0-151">Na portálu Azure na **HPE SaaS** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-151">In the Azure portal, on the **HPE SaaS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2f2a0-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_samlbase.png)

3. <span data-ttu-id="2f2a0-155">Na **HPE SaaS domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-155">On the **HPE SaaS Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_url.png)

    <span data-ttu-id="2f2a0-157">a.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-157">a.</span></span> <span data-ttu-id="2f2a0-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://login.saas.hpe.com/msg`</span><span class="sxs-lookup"><span data-stu-id="2f2a0-158">In the **Sign-on URL** textbox, type a URL as: `https://login.saas.hpe.com/msg`</span></span>

    <span data-ttu-id="2f2a0-159">b.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-159">b.</span></span> <span data-ttu-id="2f2a0-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.saas.hpe.com`</span><span class="sxs-lookup"><span data-stu-id="2f2a0-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.saas.hpe.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2f2a0-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-161">These values are not real.</span></span> <span data-ttu-id="2f2a0-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2f2a0-163">Obraťte se na [tým podpory klienta SaaS HPE](https://saas.hpe.com/en-us/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-163">Contact [HPE SaaS Client support team](https://saas.hpe.com/en-us/contact) to get these values.</span></span> 
 
4. <span data-ttu-id="2f2a0-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_certificate.png) 

5. <span data-ttu-id="2f2a0-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hpesaas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2f2a0-168">Konfigurace jednotného přihlašování na **HPE SaaS** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory HPE SaaS](https://saas.hpe.com/en-us/contact).</span><span class="sxs-lookup"><span data-stu-id="2f2a0-168">To configure single sign-on on **HPE SaaS** side, you need to send the downloaded **Metadata XML** to [HPE SaaS support team](https://saas.hpe.com/en-us/contact).</span></span> <span data-ttu-id="2f2a0-169">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="2f2a0-170">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2f2a0-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2f2a0-171">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2f2a0-172">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2f2a0-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2f2a0-173">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f2a0-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="2f2a0-174">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2f2a0-176">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f2a0-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2f2a0-177">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2f2a0-179">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2f2a0-181">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2f2a0-183">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2f2a0-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hpesaas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2f2a0-185">a.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-185">a.</span></span> <span data-ttu-id="2f2a0-186">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2f2a0-187">b.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-187">b.</span></span> <span data-ttu-id="2f2a0-188">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2f2a0-189">c.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-189">c.</span></span> <span data-ttu-id="2f2a0-190">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2f2a0-191">d.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-191">d.</span></span> <span data-ttu-id="2f2a0-192">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-192">Click **Create**.</span></span>
 
### <a name="creating-an-hpe-saas-test-user"></a><span data-ttu-id="2f2a0-193">Vytváření testovacího uživatele HPE SaaS</span><span class="sxs-lookup"><span data-stu-id="2f2a0-193">Creating an HPE SaaS test user</span></span>

<span data-ttu-id="2f2a0-194">Cílem této části je vytvoření uživatele volal Britta Simon v HPE SaaS.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-194">The objective of this section is to create a user called Britta Simon in HPE SaaS.</span></span> <span data-ttu-id="2f2a0-195">Spojte se s [tým podpory HPE SaaS](https://saas.hpe.com/en-us/contact) přidat uživatele do účtu HPE SaaS.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-195">Please work with [HPE SaaS support team](https://saas.hpe.com/en-us/contact) to add the users in the HPE SaaS account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2f2a0-196">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f2a0-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2f2a0-197">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu HPE SaaS.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HPE SaaS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2f2a0-199">**Pokud chcete přiřadit Britta Simon HPE SaaS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2f2a0-199">**To assign Britta Simon to HPE SaaS, perform the following steps:**</span></span>

1. <span data-ttu-id="2f2a0-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2f2a0-202">V seznamu aplikací vyberte **HPE SaaS**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-202">In the applications list, select **HPE SaaS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hpesaas-tutorial/tutorial_hpesaas_app.png) 

3. <span data-ttu-id="2f2a0-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2f2a0-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-206">Click **Add** button.</span></span> <span data-ttu-id="2f2a0-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2f2a0-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2f2a0-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2f2a0-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2f2a0-212">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2f2a0-212">Testing single sign-on</span></span>

<span data-ttu-id="2f2a0-213">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2f2a0-214">Když kliknete na dlaždici HPE SaaS na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci HPE SaaS.</span><span class="sxs-lookup"><span data-stu-id="2f2a0-214">When you click the HPE SaaS tile in the Access Panel, you should get automatically signed-on to your HPE SaaS application.</span></span>
<span data-ttu-id="2f2a0-215">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2f2a0-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f2a0-216">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2f2a0-216">Additional resources</span></span>

* [<span data-ttu-id="2f2a0-217">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2f2a0-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2f2a0-218">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2f2a0-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hpesaas-tutorial/tutorial_general_203.png

