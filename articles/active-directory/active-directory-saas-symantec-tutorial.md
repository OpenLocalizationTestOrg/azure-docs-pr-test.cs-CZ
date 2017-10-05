---
title: "Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS) | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Symantec webové zabezpečení služby (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 61576d3a915d209e7355e04432e586dcf66e7c5a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a><span data-ttu-id="c9e9c-103">Kurz: Azure Active Directory integrace s Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="c9e9c-103">Tutorial: Azure Active Directory integration with Symantec Web Security Service (WSS)</span></span>

<span data-ttu-id="c9e9c-104">V tomto kurzu se dozvíte, jak integrovat účtu Symantec webové zabezpečení služby (WSS) s vaším účtem Azure Active Directory (Azure AD), aby mohli WSS ověření koncový uživatel zřízenou ve službě Azure AD pomocí ověřování SAML a vynucovat pravidla zásadou na úrovni uživatele nebo skupinu.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-104">In this tutorial, you will learn how to integrate your Symantec Web Security Service (WSS) account with your Azure Active Directory (Azure AD) account so that WSS can authenticate an end user provisioned in the Azure AD using SAML authentication and enforce user or group level policy rules.</span></span>

<span data-ttu-id="c9e9c-105">Integrace Symantec webové zabezpečení služby (WSS) s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-105">Integrating Symantec Web Security Service (WSS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c9e9c-106">Spravujte všechny uživatele a skupiny používané účtu WSS z portálu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-106">Manage all of the end users and groups used by your WSS account from your Azure AD portal.</span></span> 

- <span data-ttu-id="c9e9c-107">Povolit koncoví uživatelé mohli sami ověřit v WSS pomocí svých přihlašovacích údajů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-107">Allow the end users to authenticate themselves in WSS using their Azure AD credentials.</span></span>

- <span data-ttu-id="c9e9c-108">Povolte vynucení uživatele a skupiny zásadou na úrovni pravidla definovaná v účtu WSS.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-108">Enable the enforcement of user and group level policy rules defined in your WSS account.</span></span>

<span data-ttu-id="c9e9c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9e9c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c9e9c-110">Prerequisites</span></span>

<span data-ttu-id="c9e9c-111">Ke konfiguraci integrace služby Azure AD s Symantec webové zabezpečení služby (WSS), potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-111">To configure Azure AD integration with Symantec Web Security Service (WSS), you need the following items:</span></span>

- <span data-ttu-id="c9e9c-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9e9c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9e9c-113">Účet služby pro zabezpečení webové Symantec (WSS)</span><span class="sxs-lookup"><span data-stu-id="c9e9c-113">A Symantec Web Security Service (WSS) account</span></span>

> [!NOTE]
> <span data-ttu-id="c9e9c-114">Chcete-li otestovat kroky v tomto kurzu, nedoporučujeme pomocí WSS účtu, který je aktuálně používá pro produkční účely.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-114">To test the steps in this tutorial, we do not recommend using a WSS account that is currently being used for production purpose.</span></span>

<span data-ttu-id="c9e9c-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9e9c-116">Nepoužívejte účtu WSS, který je aktuálně používá pro produkční účely pro tento test Pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-116">Do not use your WSS account that is currently being used for production purpose for this test unless it is necessary.</span></span>
- <span data-ttu-id="c9e9c-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9e9c-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="c9e9c-118">Scenario description</span></span>
<span data-ttu-id="c9e9c-119">V tomto kurzu nakonfigurujete Azure AD povolit jednotné přihlašování k WSS pomocí přihlašovacích údajů koncového uživatele, které jsou definované v účtu Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-119">In this tutorial, you will configure your Azure AD to enable single sign-on to WSS using the end user credentials defined in your Azure AD account.</span></span>
<span data-ttu-id="c9e9c-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9e9c-121">Přidání aplikace Symantec webové zabezpečení služby (WSS) z Galerie</span><span class="sxs-lookup"><span data-stu-id="c9e9c-121">Adding the Symantec Web Security Service (WSS) app from the gallery</span></span>
2. <span data-ttu-id="c9e9c-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9e9c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a><span data-ttu-id="c9e9c-123">Přidání Symantec webové zabezpečení služby (WSS) z Galerie</span><span class="sxs-lookup"><span data-stu-id="c9e9c-123">Adding Symantec Web Security Service (WSS) from the gallery</span></span>
<span data-ttu-id="c9e9c-124">Při konfiguraci integrace Symantec webové zabezpečení služby (WSS) do služby Azure AD, potřebujete přidat Symantec webové zabezpečení služby (WSS) z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-124">To configure the integration of Symantec Web Security Service (WSS) into Azure AD, you need to add Symantec Web Security Service (WSS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c9e9c-125">**Pokud chcete přidat Symantec webové zabezpečení služby (WSS) z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9e9c-125">**To add Symantec Web Security Service (WSS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c9e9c-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="c9e9c-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c9e9c-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="c9e9c-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="c9e9c-133">Do vyhledávacího pole zadejte **Symantec webové zabezpečení služby (WSS)**, vyberte **Symantec webové zabezpečení služby (WSS)** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-133">In the search box, type **Symantec Web Security Service (WSS)**, select **Symantec Web Security Service (WSS)** from result panel then click **Add** button to add the application.</span></span>

    ![Symantec webové zabezpečení služby (WSS) v seznamu výsledků](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c9e9c-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9e9c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c9e9c-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS) podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="c9e9c-136">In this section, you configure and test Azure AD single sign-on with Symantec Web Security Service (WSS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9e9c-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Symantec webové zabezpečení služby (WSS) je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Symantec Web Security Service (WSS) is to a user in Azure AD.</span></span> <span data-ttu-id="c9e9c-138">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské v Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-138">In other words, a link relationship between an Azure AD user and the related user in Symantec Web Security Service (WSS) needs to be established.</span></span>

<span data-ttu-id="c9e9c-139">V Symantec webové zabezpečení služby (WSS) přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-139">In Symantec Web Security Service (WSS), assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c9e9c-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-140">To configure and test Azure AD single sign-on with Symantec Web Security Service (WSS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c9e9c-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c9e9c-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9e9c-143">**[Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  – Pokud chcete mít protějšek Britta Simon v zabezpečení webové Symantec služby (WSS) propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-143">**[Create a Symantec Web Security Service (WSS) test user](#create-a-symantec-web-security-service-wss-test-user)** - to have a counterpart of Britta Simon in Symantec Web Security Service (WSS) that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9e9c-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9e9c-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c9e9c-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9e9c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c9e9c-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Symantec Web Security Service (WSS) application.</span></span>

<span data-ttu-id="c9e9c-148">**Pokud chcete konfigurovat Azure AD jednotné přihlašování s Symantec webové zabezpečení služby (WSS), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9e9c-148">**To configure Azure AD single sign-on with Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="c9e9c-149">Na portálu Azure na **Symantec webové zabezpečení služby (WSS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-149">In the Azure portal, on the **Symantec Web Security Service (WSS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="c9e9c-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. <span data-ttu-id="c9e9c-153">Na **Symantec webové zabezpečení služby (WSS) domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-153">On the **Symantec Web Security Service (WSS) Domain and URLs** section, perform the following steps:</span></span>

    ![Symantec webové zabezpečení služby (WSS) domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    <span data-ttu-id="c9e9c-155">a.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-155">a.</span></span> <span data-ttu-id="c9e9c-156">V **identifikátor** textovému poli, zadejte adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm`</span><span class="sxs-lookup"><span data-stu-id="c9e9c-156">In the **Identifier** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm`</span></span>

    <span data-ttu-id="c9e9c-157">b.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-157">b.</span></span> <span data-ttu-id="c9e9c-158">V **adresa URL odpovědi** textovému poli, zadejte adresu URL:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span><span class="sxs-lookup"><span data-stu-id="c9e9c-158">In the **Reply URL** textbox, type the URL: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`</span></span>

    > [!NOTE]
    > <span data-ttu-id="c9e9c-159">Obraťte se [tým podpory Symantec webové zabezpečení služby (WSS) klienta](https://www.symantec.com/contact-us) Pokud hodnoty **identifikátor** a **adresa URL odpovědi** nefungují z nějakého důvodu.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-159">Please contact the [Symantec Web Security Service (WSS) Client support team](https://www.symantec.com/contact-us) if the values for the **Identifier** and **Reply URL** are not working for some reason.</span></span>

4. <span data-ttu-id="c9e9c-160">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-160">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. <span data-ttu-id="c9e9c-162">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-162">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="c9e9c-164">Na straně Symantec webové zabezpečení služby (WSS) nakonfigurovat jednotné přihlašování, naleznete v online dokumentaci WSS.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-164">To configure single sign-on on the Symantec Web Security Service (WSS) side, refer to the WSS online documentation.</span></span> <span data-ttu-id="c9e9c-165">Stažené **soubor XML s metadaty** bude nutné importovat do portálu WSS soubor.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-165">The downloaded **Metadata XML** file will need to be imported into the WSS portal.</span></span> <span data-ttu-id="c9e9c-166">Obraťte se [tým podpory Symantec webové zabezpečení služby (WSS)](https://www.symantec.com/contact-us) Pokud potřebujete pomoc s konfigurací na portálu WSS.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-166">Contact the [Symantec Web Security Service (WSS) support team](https://www.symantec.com/contact-us) if you need assistance with the configuration on the WSS portal.</span></span>

> [!TIP]
> <span data-ttu-id="c9e9c-167">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="c9e9c-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c9e9c-168">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c9e9c-169">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9e9c-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c9e9c-170">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9e9c-170">Create an Azure AD test user</span></span>

<span data-ttu-id="c9e9c-171">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="c9e9c-173">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9e9c-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c9e9c-174">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c9e9c-176">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c9e9c-178">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c9e9c-180">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c9e9c-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c9e9c-182">a.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-182">a.</span></span> <span data-ttu-id="c9e9c-183">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9e9c-184">b.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-184">b.</span></span> <span data-ttu-id="c9e9c-185">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c9e9c-186">c.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-186">c.</span></span> <span data-ttu-id="c9e9c-187">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c9e9c-188">d.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-188">d.</span></span> <span data-ttu-id="c9e9c-189">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-189">Click **Create**.</span></span>
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a><span data-ttu-id="c9e9c-190">Vytvoření zkušebního uživatele Symantec webové zabezpečení služby (WSS)</span><span class="sxs-lookup"><span data-stu-id="c9e9c-190">Create a Symantec Web Security Service (WSS) test user</span></span>

<span data-ttu-id="c9e9c-191">V této části vytvoříte uživatele volat Britta Simon v Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-191">In this section, you create a user called Britta Simon in Symantec Web Security Service (WSS).</span></span> <span data-ttu-id="c9e9c-192">Uživatelské jméno odpovídající end je možné ručně vytvořit na portálu služby WSS nebo můžete počkat, uživatele nebo skupiny zřízené v Azure AD synchronizovány se službou portálu WSS po několika minutách (~ 15 minut).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-192">The corresponding end username can be manually created in the WSS portal or you can wait for the users/groups provisioned in the Azure AD to be synchronized to the WSS portal after a few minutes (~15 minutes).</span></span> <span data-ttu-id="c9e9c-193">Uživatelé musí být vytvořen a aktivovat dříve, než použijete jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-193">Users must be created and activated before you use single sign-on.</span></span> <span data-ttu-id="c9e9c-194">Veřejnou IP adresu počítače koncového uživatele, který se použije k procházet weby se také muset zřídit na portálu společnosti Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-194">The public IP address of the end user machine, which will be used to browse websites also need to be provisioned in the Symantec Web Security Service (WSS) portal.</span></span>

> [!NOTE]
> <span data-ttu-id="c9e9c-195">Prosím [kliknutím sem](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) získat váš počítač je veřejná IP adresa.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-195">Please [click here](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) to get your machine's public IPaddress.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c9e9c-196">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9e9c-196">Assign the Azure AD test user</span></span>

<span data-ttu-id="c9e9c-197">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k Symantec webové zabezpečení služby (WSS).</span><span class="sxs-lookup"><span data-stu-id="c9e9c-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Symantec Web Security Service (WSS).</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="c9e9c-199">**Pokud chcete přiřadit Britta Simon Symantec webové zabezpečení služby (WSS), proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="c9e9c-199">**To assign Britta Simon to Symantec Web Security Service (WSS), perform the following steps:**</span></span>

1. <span data-ttu-id="c9e9c-200">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="c9e9c-202">V seznamu aplikací vyberte **Symantec webové zabezpečení služby (WSS)**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-202">In the applications list, select **Symantec Web Security Service (WSS)**.</span></span>

    ![V seznamu aplikací na odkaz Symantec webové zabezpečení služby (WSS)](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. <span data-ttu-id="c9e9c-204">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="c9e9c-206">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-206">Click **Add** button.</span></span> <span data-ttu-id="c9e9c-207">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="c9e9c-209">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c9e9c-210">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9e9c-211">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c9e9c-212">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="c9e9c-212">Test single sign-on</span></span>

<span data-ttu-id="c9e9c-213">V této části budete testovat funkce přihlašování nyní, když jste nakonfigurovali WSS účet pro použití služby Azure AD pro ověřování SAML.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-213">In this section, you'll test the single sign-on functionality now that you've configured your WSS account to use your Azure AD for SAML authentication.</span></span>

<span data-ttu-id="c9e9c-214">Po konfiguraci webového prohlížeče proxy provoz na WSS, když otevřete ve webovém prohlížeči a zkuste a přejděte web, pak budete přesměrováni na stránku Azure přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-214">After you have configured your web browser to proxy traffic to WSS, when you open your web browser and try to browse to a site then you'll be redirected to the Azure sign-on page.</span></span> <span data-ttu-id="c9e9c-215">Zadejte přihlašovací údaje testovacího koncového uživatele, která byla zajištěna ve službě Azure AD (BrittaSimon) a přidružené heslo.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-215">Enter the credentials of the test end user that has been provisioned in the Azure AD (that is, BrittaSimon) and associated password.</span></span> <span data-ttu-id="c9e9c-216">Po ověření, budete moct přejít na web, který jste si zvolili.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-216">Once authenticated, you'll be able to browse to the website that you chose.</span></span> <span data-ttu-id="c9e9c-217">Můžete vytvořit pravidlo zásad na straně WSS zablokujete BrittaSimon procházení k určitému webu a zobrazí stránku bloku WSS při pokusu o Procházet k této lokalitě jako uživatel BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9e9c-217">Should you create a policy rule on the WSS side to block BrittaSimon from browsing to a particular site then you should see the WSS block page when you attempt to browse to that site as user BrittaSimon.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9e9c-218">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="c9e9c-218">Additional resources</span></span>

* [<span data-ttu-id="c9e9c-219">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9e9c-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9e9c-220">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9e9c-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

