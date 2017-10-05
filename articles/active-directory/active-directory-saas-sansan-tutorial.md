---
title: 'Kurz: Azure Active Directory integrace s Sansan | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Sansan."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: e1a9653d5feea910308cefabdbdfe3a6af44bbe4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a><span data-ttu-id="f7156-103">Kurz: Azure Active Directory integrace s Sansan</span><span class="sxs-lookup"><span data-stu-id="f7156-103">Tutorial: Azure Active Directory integration with Sansan</span></span>

<span data-ttu-id="f7156-104">V tomto kurzu zjistěte, jak integrovat Sansan s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f7156-104">In this tutorial, you learn how to integrate Sansan with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f7156-105">Integrace Sansan s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f7156-105">Integrating Sansan with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f7156-106">Můžete řídit ve službě Azure AD, který má přístup k Sansan</span><span class="sxs-lookup"><span data-stu-id="f7156-106">You can control in Azure AD who has access to Sansan</span></span>
- <span data-ttu-id="f7156-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Sansan (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7156-107">You can enable your users to automatically get signed-on to Sansan (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f7156-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f7156-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f7156-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f7156-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7156-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f7156-110">Prerequisites</span></span>

<span data-ttu-id="f7156-111">Konfigurace integrace Azure AD s Sansan, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f7156-111">To configure Azure AD integration with Sansan, you need the following items:</span></span>

- <span data-ttu-id="f7156-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7156-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f7156-113">Sansan jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f7156-113">A Sansan single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f7156-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7156-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f7156-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f7156-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f7156-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f7156-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f7156-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f7156-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f7156-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f7156-118">Scenario description</span></span>
<span data-ttu-id="f7156-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f7156-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f7156-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f7156-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f7156-121">Přidání Sansan z Galerie</span><span class="sxs-lookup"><span data-stu-id="f7156-121">Adding Sansan from the gallery</span></span>
2. <span data-ttu-id="f7156-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7156-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sansan-from-the-gallery"></a><span data-ttu-id="f7156-123">Přidání Sansan z Galerie</span><span class="sxs-lookup"><span data-stu-id="f7156-123">Adding Sansan from the gallery</span></span>
<span data-ttu-id="f7156-124">Při konfiguraci integrace Sansan do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Sansan z galerie.</span><span class="sxs-lookup"><span data-stu-id="f7156-124">To configure the integration of Sansan into Azure AD, you need to add Sansan from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f7156-125">**Pokud chcete přidat Sansan z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f7156-125">**To add Sansan from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f7156-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f7156-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f7156-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f7156-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f7156-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f7156-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f7156-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7156-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f7156-133">Do vyhledávacího pole zadejte **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="f7156-133">In the search box, type **Sansan**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_search.png)

5. <span data-ttu-id="f7156-135">Na panelu výsledků vyberte **Sansan**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f7156-135">In the results panel, select **Sansan**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f7156-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7156-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f7156-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sansan podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f7156-138">In this section, you configure and test Azure AD single sign-on with Sansan based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f7156-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Sansan je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f7156-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sansan is to a user in Azure AD.</span></span> <span data-ttu-id="f7156-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Sansan musí navázat.</span><span class="sxs-lookup"><span data-stu-id="f7156-140">In other words, a link relationship between an Azure AD user and the related user in Sansan needs to be established.</span></span>

<span data-ttu-id="f7156-141">V Sansan, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="f7156-141">In Sansan, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f7156-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Sansan, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f7156-142">To configure and test Azure AD single sign-on with Sansan, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f7156-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f7156-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f7156-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7156-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f7156-145">**[Vytvoření zkušebního uživatele Sansan](#creating-a-sansan-test-user)**  – Pokud chcete mít protějšek Britta Simon v Sansan propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f7156-145">**[Creating a Sansan test user](#creating-a-sansan-test-user)** - to have a counterpart of Britta Simon in Sansan that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f7156-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f7156-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f7156-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f7156-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f7156-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7156-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f7156-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Sansan.</span><span class="sxs-lookup"><span data-stu-id="f7156-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sansan application.</span></span>

<span data-ttu-id="f7156-150">**Ke konfiguraci Azure AD jednotné přihlašování s Sansan, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f7156-150">**To configure Azure AD single sign-on with Sansan, perform the following steps:**</span></span>

1. <span data-ttu-id="f7156-151">Na portálu Azure na **Sansan** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f7156-151">In the Azure portal, on the **Sansan** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f7156-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f7156-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_samlbase.png)

3. <span data-ttu-id="f7156-155">Na **Sansan domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f7156-155">On the **Sansan Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_url.png)

    <span data-ttu-id="f7156-157">a.</span><span class="sxs-lookup"><span data-stu-id="f7156-157">a.</span></span> <span data-ttu-id="f7156-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="f7156-158">In the **Sign-on URL** textbox, type a URL using the following patterns:</span></span> 
    
    | <span data-ttu-id="f7156-159">Prostředí</span><span class="sxs-lookup"><span data-stu-id="f7156-159">Environment</span></span> | <span data-ttu-id="f7156-160">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="f7156-160">URL</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="f7156-161">Webové PC</span><span class="sxs-lookup"><span data-stu-id="f7156-161">PC web</span></span> |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | <span data-ttu-id="f7156-162">Nativní mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="f7156-162">Native Mobile app</span></span> |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | <span data-ttu-id="f7156-163">Nastavení mobilní prohlížeče</span><span class="sxs-lookup"><span data-stu-id="f7156-163">Mobile browser settings</span></span> |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    <span data-ttu-id="f7156-164">b.</span><span class="sxs-lookup"><span data-stu-id="f7156-164">b.</span></span> <span data-ttu-id="f7156-165">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="f7156-165">In the **Identifier** textbox, type a URL using the following patterns:</span></span>
    | <span data-ttu-id="f7156-166">Prostředí</span><span class="sxs-lookup"><span data-stu-id="f7156-166">Environment</span></span>             | <span data-ttu-id="f7156-167">ADRESA URL</span><span class="sxs-lookup"><span data-stu-id="f7156-167">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="f7156-168">Webové PC</span><span class="sxs-lookup"><span data-stu-id="f7156-168">PC web</span></span>                  | `https://ap.sansan.com/v/saml2/<company name>`|
    | <span data-ttu-id="f7156-169">Nativní mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="f7156-169">Native Mobile app</span></span>       | `https://internal.api.sansan.com/saml2/<company name>` |
    | <span data-ttu-id="f7156-170">Nastavení mobilní prohlížeče</span><span class="sxs-lookup"><span data-stu-id="f7156-170">Mobile browser settings</span></span> | `https://ap.sansan.com/s/saml2/<company name>` |

    > [!NOTE] 
    > <span data-ttu-id="f7156-171">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="f7156-171">These values are not real.</span></span> <span data-ttu-id="f7156-172">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="f7156-172">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f7156-173">Obraťte se na [tým podpory Sansan klienta](https://www.sansan.com/form/contact) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f7156-173">Contact [Sansan Client support team](https://www.sansan.com/form/contact) to get these values.</span></span> 

4. <span data-ttu-id="f7156-174">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="f7156-174">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_certificate.png) 

5. <span data-ttu-id="f7156-176">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7156-176">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f7156-178">Na **Sansan konfigurace** klikněte na tlačítko **konfigurace Sansan** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f7156-178">On the **Sansan Configuration** section, click **Configure Sansan** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f7156-179">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="f7156-179">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_configure.png) 

7. <span data-ttu-id="f7156-181">Konfigurace jednotného přihlašování na **Sansan** straně, budete muset odeslat stažené **certifikát**, **Sign-Out URL**, **SAML Entity ID**, a **SAML jeden přihlašování adresa URL služby** k [tým podpory Sansan](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="f7156-181">To configure single sign-on on **Sansan** side, you need to send the downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** to [Sansan support team](https://www.sansan.com/form/contact).</span></span> <span data-ttu-id="f7156-182">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="f7156-182">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

>[!NOTE]
><span data-ttu-id="f7156-183">Nastavení prohlížeče počítače pracovních také pro mobilní aplikace a společně s počítači webový prohlížeč pro mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="f7156-183">PC browser setting also work for Mobile app and Mobile browser along with PC web.</span></span>  

> [!TIP]
> <span data-ttu-id="f7156-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f7156-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f7156-185">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f7156-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f7156-186">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f7156-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f7156-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7156-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="f7156-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7156-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f7156-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f7156-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f7156-191">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f7156-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f7156-193">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f7156-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f7156-195">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7156-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f7156-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f7156-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f7156-199">a.</span><span class="sxs-lookup"><span data-stu-id="f7156-199">a.</span></span> <span data-ttu-id="f7156-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f7156-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f7156-201">b.</span><span class="sxs-lookup"><span data-stu-id="f7156-201">b.</span></span> <span data-ttu-id="f7156-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f7156-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f7156-203">c.</span><span class="sxs-lookup"><span data-stu-id="f7156-203">c.</span></span> <span data-ttu-id="f7156-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f7156-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f7156-205">d.</span><span class="sxs-lookup"><span data-stu-id="f7156-205">d.</span></span> <span data-ttu-id="f7156-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f7156-206">Click **Create**.</span></span>
 
### <a name="creating-a-sansan-test-user"></a><span data-ttu-id="f7156-207">Vytvoření zkušebního uživatele Sansan</span><span class="sxs-lookup"><span data-stu-id="f7156-207">Creating a Sansan test user</span></span>

<span data-ttu-id="f7156-208">V této části vytvoříte volal Britta Simon v SanSan uživatele.</span><span class="sxs-lookup"><span data-stu-id="f7156-208">In this section, you create a user called Britta Simon in SanSan.</span></span> <span data-ttu-id="f7156-209">SanSan aplikace, musí uživatel zřídit v aplikaci před provedením jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f7156-209">SanSan application needs the user to be provisioned in the application before doing SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="f7156-210">Pokud potřebujete ručně vytvořte uživatele nebo dávky uživatelů, budete muset kontaktovat [tým podpory Sansan](https://www.sansan.com/form/contact).</span><span class="sxs-lookup"><span data-stu-id="f7156-210">If you need to create a user manually or batch of users, you need to contact the [Sansan support team](https://www.sansan.com/form/contact).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f7156-211">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7156-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f7156-212">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Sansan.</span><span class="sxs-lookup"><span data-stu-id="f7156-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sansan.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f7156-214">**Pokud chcete přiřadit Britta Simon Sansan, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f7156-214">**To assign Britta Simon to Sansan, perform the following steps:**</span></span>

1. <span data-ttu-id="f7156-215">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f7156-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f7156-217">V seznamu aplikací vyberte **Sansan**.</span><span class="sxs-lookup"><span data-stu-id="f7156-217">In the applications list, select **Sansan**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_app.png) 

3. <span data-ttu-id="f7156-219">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f7156-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f7156-221">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f7156-221">Click **Add** button.</span></span> <span data-ttu-id="f7156-222">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7156-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f7156-224">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f7156-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f7156-225">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7156-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f7156-226">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f7156-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f7156-227">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f7156-227">Testing single sign-on</span></span>

<span data-ttu-id="f7156-228">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f7156-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f7156-229">Když kliknete na dlaždici Sansan na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Sansan.</span><span class="sxs-lookup"><span data-stu-id="f7156-229">When you click the Sansan tile in the Access Panel, you should get automatically signed-on to your Sansan application.</span></span>
<span data-ttu-id="f7156-230">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f7156-230">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7156-231">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f7156-231">Additional resources</span></span>

* [<span data-ttu-id="f7156-232">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f7156-232">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f7156-233">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f7156-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
