---
title: 'Kurz: Azure Active Directory integrace s Ceridian Dayforce HCM | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Ceridian Dayforce HCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: b2ea3d92f233dab5bd6814e4875f881117eac8e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a><span data-ttu-id="16d90-103">Kurz: Azure Active Directory integrace s Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="16d90-103">Tutorial: Azure Active Directory integration with Ceridian Dayforce HCM</span></span>

<span data-ttu-id="16d90-104">V tomto kurzu zjistěte, jak integrovat Ceridian Dayforce HCM s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="16d90-104">In this tutorial, you learn how to integrate Ceridian Dayforce HCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="16d90-105">Integrace Ceridian Dayforce HCM s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="16d90-105">Integrating Ceridian Dayforce HCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="16d90-106">Můžete ovládat ve službě Azure AD, který má přístup k Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="16d90-106">You can control in Azure AD who has access to Ceridian Dayforce HCM.</span></span>
- <span data-ttu-id="16d90-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Ceridian Dayforce HCM (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16d90-107">You can enable your users to automatically get signed-on to Ceridian Dayforce HCM (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="16d90-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="16d90-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="16d90-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="16d90-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16d90-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="16d90-110">Prerequisites</span></span>

<span data-ttu-id="16d90-111">Konfigurace integrace Azure AD s Ceridian Dayforce HCM, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="16d90-111">To configure Azure AD integration with Ceridian Dayforce HCM, you need the following items:</span></span>

- <span data-ttu-id="16d90-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="16d90-112">An Azure AD subscription</span></span>
- <span data-ttu-id="16d90-113">Ceridian Dayforce HCM jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="16d90-113">A Ceridian Dayforce HCM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="16d90-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="16d90-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="16d90-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="16d90-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="16d90-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="16d90-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="16d90-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="16d90-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="16d90-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="16d90-118">Scenario description</span></span>
<span data-ttu-id="16d90-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="16d90-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="16d90-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="16d90-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="16d90-121">Přidání Ceridian Dayforce HCM z Galerie</span><span class="sxs-lookup"><span data-stu-id="16d90-121">Adding Ceridian Dayforce HCM from the gallery</span></span>
2. <span data-ttu-id="16d90-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="16d90-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a><span data-ttu-id="16d90-123">Přidání Ceridian Dayforce HCM z Galerie</span><span class="sxs-lookup"><span data-stu-id="16d90-123">Adding Ceridian Dayforce HCM from the gallery</span></span>
<span data-ttu-id="16d90-124">Při konfiguraci integrace Ceridian Dayforce HCM do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Ceridian Dayforce HCM z galerie.</span><span class="sxs-lookup"><span data-stu-id="16d90-124">To configure the integration of Ceridian Dayforce HCM into Azure AD, you need to add Ceridian Dayforce HCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="16d90-125">**Pokud chcete přidat Ceridian Dayforce HCM z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="16d90-125">**To add Ceridian Dayforce HCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="16d90-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="16d90-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="16d90-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="16d90-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="16d90-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="16d90-129">Then go to **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="16d90-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="16d90-133">Do vyhledávacího pole zadejte **Ceridian Dayforce HCM**, vyberte **Ceridian Dayforce HCM** z panelu výsledků klikněte **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="16d90-133">In the search box, type **Ceridian Dayforce HCM**, select **Ceridian Dayforce HCM** from result panel then click **Add** button to add the application.</span></span>

    ![Ceridian Dayforce HCM v seznamu výsledků](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="16d90-135">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="16d90-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="16d90-136">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Ceridian Dayforce HCM podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="16d90-136">In this section, you configure and test Azure AD single sign-on with Ceridian Dayforce HCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="16d90-137">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Ceridian Dayforce HCM je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="16d90-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Ceridian Dayforce HCM is to a user in Azure AD.</span></span> <span data-ttu-id="16d90-138">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Ceridian Dayforce HCM musí navázat.</span><span class="sxs-lookup"><span data-stu-id="16d90-138">In other words, a link relationship between an Azure AD user and the related user in Ceridian Dayforce HCM needs to be established.</span></span>

<span data-ttu-id="16d90-139">V Ceridian Dayforce HCM, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="16d90-139">In Ceridian Dayforce HCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="16d90-140">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Ceridian Dayforce HCM, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="16d90-140">To configure and test Azure AD single sign-on with Ceridian Dayforce HCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="16d90-141">**[Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="16d90-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="16d90-142">**[Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="16d90-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="16d90-143">**[Vytvoření zkušebního uživatele Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user)**  – Pokud chcete mít protějšek Britta Simon v HCM Dayforce Ceridian, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="16d90-143">**[Create a Ceridian Dayforce HCM test user](#create-a-ceridian-dayforce-hcm-test-user)** - to have a counterpart of Britta Simon in Ceridian Dayforce HCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="16d90-144">**[Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="16d90-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="16d90-145">**[Test jednotného přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="16d90-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="16d90-146">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="16d90-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="16d90-147">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="16d90-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Ceridian Dayforce HCM application.</span></span>

<span data-ttu-id="16d90-148">**Ke konfiguraci Azure AD jednotné přihlašování s Ceridian Dayforce HCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="16d90-148">**To configure Azure AD single sign-on with Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="16d90-149">Na portálu Azure na **Ceridian Dayforce HCM** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="16d90-149">In the Azure portal, on the **Ceridian Dayforce HCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="16d90-151">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="16d90-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. <span data-ttu-id="16d90-153">Na **Ceridian Dayforce HCM domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="16d90-153">On the **Ceridian Dayforce HCM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    <span data-ttu-id="16d90-155">a.</span><span class="sxs-lookup"><span data-stu-id="16d90-155">a.</span></span> <span data-ttu-id="16d90-156">V **přihlašovací adresa URL** textové pole, zadejte adresu URL používá uživatelům přihlášení do aplikace Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="16d90-156">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Ceridian Dayforce HCM application.</span></span>
    
    | <span data-ttu-id="16d90-157">Prostředí</span><span class="sxs-lookup"><span data-stu-id="16d90-157">Environment</span></span> | <span data-ttu-id="16d90-158">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="16d90-158">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="16d90-159">Pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="16d90-159">For production</span></span> | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | <span data-ttu-id="16d90-160">Pro test</span><span class="sxs-lookup"><span data-stu-id="16d90-160">For test</span></span> | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    <span data-ttu-id="16d90-161">b.</span><span class="sxs-lookup"><span data-stu-id="16d90-161">b.</span></span> <span data-ttu-id="16d90-162">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="16d90-162">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    
    | <span data-ttu-id="16d90-163">Prostředí</span><span class="sxs-lookup"><span data-stu-id="16d90-163">Environment</span></span> | <span data-ttu-id="16d90-164">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="16d90-164">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="16d90-165">Pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="16d90-165">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp` |
    | <span data-ttu-id="16d90-166">Pro test</span><span class="sxs-lookup"><span data-stu-id="16d90-166">For test</span></span> | `https://fs-test.dayforcehcm.com/sp` |
    
    <span data-ttu-id="16d90-167">c.</span><span class="sxs-lookup"><span data-stu-id="16d90-167">c.</span></span> <span data-ttu-id="16d90-168">V **adresa URL odpovědi** textové pole, zadejte adresu URL používá Azure AD při odesílání odpovědi.</span><span class="sxs-lookup"><span data-stu-id="16d90-168">In the **Reply URL** textbox, type the URL used by Azure AD to post the response.</span></span>
    
    | <span data-ttu-id="16d90-169">Prostředí</span><span class="sxs-lookup"><span data-stu-id="16d90-169">Environment</span></span> | <span data-ttu-id="16d90-170">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="16d90-170">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="16d90-171">Pro produkční prostředí</span><span class="sxs-lookup"><span data-stu-id="16d90-171">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | <span data-ttu-id="16d90-172">Pro test</span><span class="sxs-lookup"><span data-stu-id="16d90-172">For test</span></span> | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > <span data-ttu-id="16d90-173">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="16d90-173">These values are not real.</span></span> <span data-ttu-id="16d90-174">Tyto hodnoty aktualizujte se skutečným identifikátorem, adresa URL odpovědi a přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="16d90-174">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="16d90-175">Obraťte se na [tým podpory Ceridian Dayforce HCM klienta](https://www.ceridian.com/contact-us/index.html) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="16d90-175">Contact [Ceridian Dayforce HCM Client support team](https://www.ceridian.com/contact-us/index.html) to get these values.</span></span>

4. <span data-ttu-id="16d90-176">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="16d90-176">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. <span data-ttu-id="16d90-178">Aplikace Ceridian Dayforce HCM očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="16d90-178">Your Ceridian Dayforce HCM application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="16d90-179">Práce s [tým podpory Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html) nejprve k identifikaci uživatele správný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="16d90-179">Work with [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) first to identify the correct user identifier.</span></span> <span data-ttu-id="16d90-180">Společnost Microsoft doporučuje používat **"název"** atribut jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="16d90-180">Microsoft recommends using the **"name"** attribute as user identifier.</span></span> <span data-ttu-id="16d90-181">Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="16d90-181">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="16d90-182">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="16d90-182">The following screenshot shows an example for this.</span></span>  

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. <span data-ttu-id="16d90-184">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="16d90-184">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="16d90-185">Název atributu</span><span class="sxs-lookup"><span data-stu-id="16d90-185">Attribute Name</span></span>  | <span data-ttu-id="16d90-186">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="16d90-186">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="16d90-187">jméno</span><span class="sxs-lookup"><span data-stu-id="16d90-187">name</span></span>  | <span data-ttu-id="16d90-188">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="16d90-188">user.extensionattribute2</span></span> |    

    <span data-ttu-id="16d90-189">a.</span><span class="sxs-lookup"><span data-stu-id="16d90-189">a.</span></span> <span data-ttu-id="16d90-190">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-190">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="16d90-193">b.</span><span class="sxs-lookup"><span data-stu-id="16d90-193">b.</span></span> <span data-ttu-id="16d90-194">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="16d90-194">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="16d90-195">c.</span><span class="sxs-lookup"><span data-stu-id="16d90-195">c.</span></span> <span data-ttu-id="16d90-196">V **hodnotu** vyberte atribut uživatele, kterou chcete použít týkající se vaší implementace.</span><span class="sxs-lookup"><span data-stu-id="16d90-196">In the **Value** list, select the user attribute you want to use for your implementation.</span></span>
    <span data-ttu-id="16d90-197">Například pokud chcete použít EmployeeID jako uživatel jedinečný identifikátor a ukládaly hodnota atributu v ExtensionAttribute2, pak vyberte **user.extensionattribute2**.</span><span class="sxs-lookup"><span data-stu-id="16d90-197">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select **user.extensionattribute2**.</span></span>
    
    <span data-ttu-id="16d90-198">d.</span><span class="sxs-lookup"><span data-stu-id="16d90-198">d.</span></span> <span data-ttu-id="16d90-199">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="16d90-199">Click **Ok**.</span></span>

7. <span data-ttu-id="16d90-200">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16d90-200">Click **Save** button.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="16d90-202">Na **Ceridian Dayforce HCM konfigurace** klikněte na tlačítko **konfigurace HCM Dayforce Ceridian** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-202">On the **Ceridian Dayforce HCM Configuration** section, click **Configure Ceridian Dayforce HCM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="16d90-203">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="16d90-203">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurace HCM Ceridian Dayforce](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. <span data-ttu-id="16d90-205">Konfigurace jednotného přihlašování na **Ceridian Dayforce HCM** straně, budete muset odeslat stažené **soubor XML s metadaty** a **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html).</span><span class="sxs-lookup"><span data-stu-id="16d90-205">To configure single sign-on on **Ceridian Dayforce HCM** side, you need to send the downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html).</span></span>

> [!TIP]
> <span data-ttu-id="16d90-206">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="16d90-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="16d90-207">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="16d90-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="16d90-208">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="16d90-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="16d90-209">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="16d90-209">Create an Azure AD test user</span></span>

<span data-ttu-id="16d90-210">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="16d90-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Vytvořit testovací uživatele Azure AD][100]

<span data-ttu-id="16d90-212">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="16d90-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="16d90-213">Na portálu Azure, v levém podokně klikněte **Azure Active Directory** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16d90-213">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="16d90-215">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="16d90-215">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="16d90-217">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-217">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Tlačítko Přidat](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="16d90-219">V **uživatele** dialogové okno pole, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="16d90-219">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogové okno uživatele](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    <span data-ttu-id="16d90-221">a.</span><span class="sxs-lookup"><span data-stu-id="16d90-221">a.</span></span> <span data-ttu-id="16d90-222">V **název** zadejte **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="16d90-222">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="16d90-223">b.</span><span class="sxs-lookup"><span data-stu-id="16d90-223">b.</span></span> <span data-ttu-id="16d90-224">V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="16d90-224">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="16d90-225">c.</span><span class="sxs-lookup"><span data-stu-id="16d90-225">c.</span></span> <span data-ttu-id="16d90-226">Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.</span><span class="sxs-lookup"><span data-stu-id="16d90-226">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="16d90-227">d.</span><span class="sxs-lookup"><span data-stu-id="16d90-227">d.</span></span> <span data-ttu-id="16d90-228">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="16d90-228">Click **Create**.</span></span>
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a><span data-ttu-id="16d90-229">Vytvoření zkušebního uživatele Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="16d90-229">Create a Ceridian Dayforce HCM test user</span></span>

<span data-ttu-id="16d90-230">Cílem této části je vytvoření uživatele v Ceridian Dayforce HCM nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="16d90-230">The objective of this section is to create a user called Britta Simon in Ceridian Dayforce HCM.</span></span> <span data-ttu-id="16d90-231">Pracovat [tým podpory Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html) získat uživatelé přidaní do Ceridian Dayforce HCM aplikace.</span><span class="sxs-lookup"><span data-stu-id="16d90-231">Work with the [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) to get users added in the Ceridian Dayforce HCM application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="16d90-232">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="16d90-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="16d90-233">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="16d90-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ceridian Dayforce HCM.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="16d90-235">**Pokud chcete přiřadit Britta Simon Ceridian Dayforce HCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="16d90-235">**To assign Britta Simon to Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="16d90-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="16d90-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="16d90-238">V seznamu aplikací vyberte **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="16d90-238">In the applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. <span data-ttu-id="16d90-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="16d90-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="16d90-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16d90-242">Click **Add** button.</span></span> <span data-ttu-id="16d90-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="16d90-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="16d90-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="16d90-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="16d90-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-247">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="16d90-248">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="16d90-248">Assign the Azure AD test user</span></span>

<span data-ttu-id="16d90-249">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="16d90-249">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ceridian Dayforce HCM.</span></span>

![Přiřadit role uživatele][200] 

<span data-ttu-id="16d90-251">**Pokud chcete přiřadit Britta Simon Ceridian Dayforce HCM, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="16d90-251">**To assign Britta Simon to Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="16d90-252">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="16d90-252">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="16d90-254">V seznamu aplikací vyberte **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="16d90-254">In the applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![V seznamu aplikací na Ceridian Dayforce HCM odkaz](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. <span data-ttu-id="16d90-256">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="16d90-256">In the menu on the left, click **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202]

4. <span data-ttu-id="16d90-258">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="16d90-258">Click **Add** button.</span></span> <span data-ttu-id="16d90-259">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="16d90-261">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="16d90-261">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="16d90-262">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="16d90-263">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="16d90-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="16d90-264">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="16d90-264">Test single sign-on</span></span>

<span data-ttu-id="16d90-265">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="16d90-265">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="16d90-266">Když kliknete na dlaždici Ceridian Dayforce HCM na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="16d90-266">When you click the Ceridian Dayforce HCM tile in the Access Panel, you should get automatically signed-on to your Ceridian Dayforce HCM application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="16d90-267">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="16d90-267">Additional resources</span></span>

* [<span data-ttu-id="16d90-268">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="16d90-268">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="16d90-269">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="16d90-269">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

