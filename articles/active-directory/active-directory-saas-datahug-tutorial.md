---
title: 'Kurz: Azure Active Directory integrace s Datahug | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Datahug."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: ec431dd5ccfa53e4b975e46da247704dd1e15c2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a><span data-ttu-id="338c0-103">Kurz: Azure Active Directory integrace s Datahug</span><span class="sxs-lookup"><span data-stu-id="338c0-103">Tutorial: Azure Active Directory integration with Datahug</span></span>

<span data-ttu-id="338c0-104">V tomto kurzu zjistěte, jak integrovat Datahug s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="338c0-104">In this tutorial, you learn how to integrate Datahug with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="338c0-105">Integrace Datahug s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="338c0-105">Integrating Datahug with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="338c0-106">Můžete řídit ve službě Azure AD, který má přístup k Datahug</span><span class="sxs-lookup"><span data-stu-id="338c0-106">You can control in Azure AD who has access to Datahug</span></span>
- <span data-ttu-id="338c0-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Datahug (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="338c0-107">You can enable your users to automatically get signed-on to Datahug (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="338c0-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="338c0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="338c0-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="338c0-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="338c0-110">[Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="338c0-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="338c0-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="338c0-111">Prerequisites</span></span>

<span data-ttu-id="338c0-112">Konfigurace integrace Azure AD s Datahug, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="338c0-112">To configure Azure AD integration with Datahug, you need the following items:</span></span>

- <span data-ttu-id="338c0-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="338c0-113">An Azure AD subscription</span></span>
- <span data-ttu-id="338c0-114">Datahug jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="338c0-114">A Datahug single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="338c0-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="338c0-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="338c0-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="338c0-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="338c0-117">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="338c0-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="338c0-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="338c0-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="338c0-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="338c0-119">Scenario description</span></span>
<span data-ttu-id="338c0-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="338c0-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="338c0-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="338c0-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="338c0-122">Přidání Datahug z Galerie</span><span class="sxs-lookup"><span data-stu-id="338c0-122">Adding Datahug from the gallery</span></span>
2. <span data-ttu-id="338c0-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="338c0-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-datahug-from-the-gallery"></a><span data-ttu-id="338c0-124">Přidání Datahug z Galerie</span><span class="sxs-lookup"><span data-stu-id="338c0-124">Adding Datahug from the gallery</span></span>
<span data-ttu-id="338c0-125">Při konfiguraci integrace Datahug do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Datahug z galerie.</span><span class="sxs-lookup"><span data-stu-id="338c0-125">To configure the integration of Datahug into Azure AD, you need to add Datahug from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="338c0-126">**Pokud chcete přidat Datahug z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="338c0-126">**To add Datahug from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="338c0-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="338c0-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="338c0-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="338c0-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="338c0-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="338c0-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="338c0-132">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="338c0-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="338c0-134">Do vyhledávacího pole zadejte **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="338c0-134">In the search box, type **Datahug**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. <span data-ttu-id="338c0-136">Na panelu výsledků vyberte **Datahug**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="338c0-136">In the results panel, select **Datahug**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="338c0-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="338c0-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="338c0-139">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Datahug podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="338c0-139">In this section, you configure and test Azure AD single sign-on with Datahug based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="338c0-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Datahug je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="338c0-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Datahug is to a user in Azure AD.</span></span> <span data-ttu-id="338c0-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Datahug musí navázat.</span><span class="sxs-lookup"><span data-stu-id="338c0-141">In other words, a link relationship between an Azure AD user and the related user in Datahug needs to be established.</span></span>

<span data-ttu-id="338c0-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v Datahug.</span><span class="sxs-lookup"><span data-stu-id="338c0-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Datahug.</span></span>

<span data-ttu-id="338c0-143">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Datahug, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="338c0-143">To configure and test Azure AD single sign-on with Datahug, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="338c0-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="338c0-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="338c0-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="338c0-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="338c0-146">**[Vytvoření zkušebního uživatele Datahug](#creating-a-datahug-test-user)**  – Pokud chcete mít protějšek Britta Simon v Datahug propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="338c0-146">**[Creating a Datahug test user](#creating-a-datahug-test-user)** - to have a counterpart of Britta Simon in Datahug that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="338c0-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="338c0-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="338c0-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="338c0-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="338c0-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="338c0-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="338c0-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Datahug.</span><span class="sxs-lookup"><span data-stu-id="338c0-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Datahug application.</span></span>

<span data-ttu-id="338c0-151">**Ke konfiguraci Azure AD jednotné přihlašování s Datahug, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="338c0-151">**To configure Azure AD single sign-on with Datahug, perform the following steps:**</span></span>

1. <span data-ttu-id="338c0-152">Na portálu Azure na **Datahug** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="338c0-152">In the Azure portal, on the **Datahug** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="338c0-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="338c0-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. <span data-ttu-id="338c0-156">Na **Datahug domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="338c0-156">On the **Datahug Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    <span data-ttu-id="338c0-158">a.</span><span class="sxs-lookup"><span data-stu-id="338c0-158">a.</span></span> <span data-ttu-id="338c0-159">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://apps.datahug.com/identity/<uniqueID>`</span><span class="sxs-lookup"><span data-stu-id="338c0-159">In the **Identifier** textbox, type a URL using the following pattern: `https://apps.datahug.com/identity/<uniqueID>`</span></span>

    <span data-ttu-id="338c0-160">b.</span><span class="sxs-lookup"><span data-stu-id="338c0-160">b.</span></span> <span data-ttu-id="338c0-161">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://apps.datahug.com/identity/<uniqueID>/acs`</span><span class="sxs-lookup"><span data-stu-id="338c0-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://apps.datahug.com/identity/<uniqueID>/acs`</span></span>

4. <span data-ttu-id="338c0-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="338c0-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="338c0-163">Pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="338c0-163">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    <span data-ttu-id="338c0-165">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL jako:`https://apps.datahug.com/`</span><span class="sxs-lookup"><span data-stu-id="338c0-165">In the **Sign-on URL** textbox, type a URL as: `https://apps.datahug.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="338c0-166">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="338c0-166">These values are not the real.</span></span> <span data-ttu-id="338c0-167">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="338c0-167">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="338c0-168">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátor a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="338c0-168">Here we suggest you to use the unique value of string in the Identifier and Reply URL.</span></span> <span data-ttu-id="338c0-169">Obraťte se na [tým podpory Datahug klienta](http://datahug.com/about/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="338c0-169">Contact [Datahug Client support team](http://datahug.com/about/contact-us/) to get these values.</span></span> 

5. <span data-ttu-id="338c0-170">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="338c0-170">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  <span data-ttu-id="338c0-172">Zkontrolujte **"Zobrazit upřesňující nastavení podpisový certifikát"** a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="338c0-172">Check **“Show advanced certificate signing settings”** and perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    <span data-ttu-id="338c0-174">a.</span><span class="sxs-lookup"><span data-stu-id="338c0-174">a.</span></span> <span data-ttu-id="338c0-175">V **podepisování možnost**, vyberte **kontrolního výrazu SAML přihlašovací**.</span><span class="sxs-lookup"><span data-stu-id="338c0-175">In **Signing Option**, select **Sign SAML assertion**.</span></span>
    
    <span data-ttu-id="338c0-176">b.</span><span class="sxs-lookup"><span data-stu-id="338c0-176">b.</span></span> <span data-ttu-id="338c0-177">V **podpisového algoritmu**, vyberte **SHA1**.</span><span class="sxs-lookup"><span data-stu-id="338c0-177">In **Signing algorithm**, select **SHA1**.</span></span>
 
7. <span data-ttu-id="338c0-178">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="338c0-178">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="338c0-180">Na **Datahug konfigurace** klikněte na tlačítko **konfigurace Datahug** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="338c0-180">On the **Datahug Configuration** section, click **Configure Datahug** to open **Configure sign-on** window.</span></span> <span data-ttu-id="338c0-181">Kopírování **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="338c0-181">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. <span data-ttu-id="338c0-183">Konfigurace jednotného přihlašování na **Datahug** straně, budete muset odeslat stažené **soubor XML s metadaty**, **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby**  k [Datahug podporu](http://datahug.com/about/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="338c0-183">To configure single sign-on on **Datahug** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** to [Datahug support](http://datahug.com/about/contact-us/).</span></span> <span data-ttu-id="338c0-184">Nastavené tuto aplikaci tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="338c0-184">They set this application up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="338c0-185">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="338c0-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="338c0-186">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="338c0-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="338c0-187">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="338c0-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="338c0-188">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="338c0-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="338c0-189">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="338c0-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="338c0-191">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="338c0-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="338c0-192">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="338c0-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="338c0-194">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="338c0-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="338c0-196">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="338c0-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="338c0-198">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="338c0-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="338c0-200">a.</span><span class="sxs-lookup"><span data-stu-id="338c0-200">a.</span></span> <span data-ttu-id="338c0-201">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="338c0-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="338c0-202">b.</span><span class="sxs-lookup"><span data-stu-id="338c0-202">b.</span></span> <span data-ttu-id="338c0-203">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="338c0-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="338c0-204">c.</span><span class="sxs-lookup"><span data-stu-id="338c0-204">c.</span></span> <span data-ttu-id="338c0-205">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="338c0-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="338c0-206">d.</span><span class="sxs-lookup"><span data-stu-id="338c0-206">d.</span></span> <span data-ttu-id="338c0-207">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="338c0-207">Click **Create**.</span></span>
 
### <a name="creating-a-datahug-test-user"></a><span data-ttu-id="338c0-208">Vytvoření zkušebního uživatele Datahug</span><span class="sxs-lookup"><span data-stu-id="338c0-208">Creating a Datahug test user</span></span>

<span data-ttu-id="338c0-209">Pokud chcete povolit uživatelům Azure AD přihlášení k Datahug, musí být zřízená do Datahug.</span><span class="sxs-lookup"><span data-stu-id="338c0-209">To enable Azure AD users to log in to Datahug, they must be provisioned into Datahug.</span></span>  
<span data-ttu-id="338c0-210">Při zřizování Datahug, je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="338c0-210">When Datahug, provisioning is a manual task.</span></span>

<span data-ttu-id="338c0-211">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="338c0-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="338c0-212">Přihlaste se k serveru vaší společnosti Datahug jako správce.</span><span class="sxs-lookup"><span data-stu-id="338c0-212">Log in to your Datahug company site as an administrator.</span></span>

2. <span data-ttu-id="338c0-213">Najeďte myší **ozubené kolo** v pravém horním a klikněte na **nastavení**</span><span class="sxs-lookup"><span data-stu-id="338c0-213">Hover over the **cog** in the top right-hand corner and click **Settings**</span></span>
   
   ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/1.png)

3. <span data-ttu-id="338c0-215">Zvolte **osoby** a klikněte na tlačítko **přidat uživatele** karta</span><span class="sxs-lookup"><span data-stu-id="338c0-215">Choose **People** and click the **Add Users** tab</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/2.png)

4. <span data-ttu-id="338c0-217">Zadejte e-mailu osoby, kterou chcete vytvořit účet pro a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="338c0-217">Type the email of the person you would like to create an account for and click **Add**.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > <span data-ttu-id="338c0-219">Můžete odeslat e-mailu registrace uživatele tak, že vyberete **odeslání Uvítacího e-mailu** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="338c0-219">You can send registration mail to user by selecting **Send welcome email** checkbox.</span></span>  
    > <span data-ttu-id="338c0-220">Při vytváření účtu služby Salesforce Neodesílat Uvítacího e-mailu.</span><span class="sxs-lookup"><span data-stu-id="338c0-220">If you are creating an account for Salesforce do not send the welcome email.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="338c0-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="338c0-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="338c0-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Datahug.</span><span class="sxs-lookup"><span data-stu-id="338c0-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Datahug.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="338c0-224">**Pokud chcete přiřadit Britta Simon Datahug, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="338c0-224">**To assign Britta Simon to Datahug, perform the following steps:**</span></span>

1. <span data-ttu-id="338c0-225">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="338c0-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="338c0-227">V seznamu aplikací vyberte **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="338c0-227">In the applications list, select **Datahug**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. <span data-ttu-id="338c0-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="338c0-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="338c0-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="338c0-231">Click **Add** button.</span></span> <span data-ttu-id="338c0-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="338c0-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="338c0-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="338c0-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="338c0-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="338c0-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="338c0-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="338c0-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="338c0-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="338c0-237">Testing single sign-on</span></span>

<span data-ttu-id="338c0-238">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="338c0-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="338c0-239">Když kliknete na dlaždici Datahug na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Datahug.</span><span class="sxs-lookup"><span data-stu-id="338c0-239">When you click the Datahug tile in the Access Panel, you should get automatically signed-on to your Datahug application.</span></span> <span data-ttu-id="338c0-240">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="338c0-240">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="338c0-241">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="338c0-241">Additional resources</span></span>

* [<span data-ttu-id="338c0-242">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="338c0-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="338c0-243">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="338c0-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

