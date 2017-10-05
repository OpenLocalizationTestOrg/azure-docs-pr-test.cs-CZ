---
title: 'Kurz: Azure Active Directory integrace s Samanage | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Samanage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: c54dbe407145a29a712acc3c0fb549a38ac26bed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a><span data-ttu-id="a5f60-103">Kurz: Azure Active Directory integrace s Samanage</span><span class="sxs-lookup"><span data-stu-id="a5f60-103">Tutorial: Azure Active Directory integration with Samanage</span></span>

<span data-ttu-id="a5f60-104">V tomto kurzu zjistěte, jak integrovat Samanage s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a5f60-104">In this tutorial, you learn how to integrate Samanage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a5f60-105">Integrace Samanage s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a5f60-105">Integrating Samanage with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a5f60-106">Můžete řídit ve službě Azure AD, který má přístup k Samanage</span><span class="sxs-lookup"><span data-stu-id="a5f60-106">You can control in Azure AD who has access to Samanage</span></span>
- <span data-ttu-id="a5f60-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Samanage (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5f60-107">You can enable your users to automatically get signed-on to Samanage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a5f60-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a5f60-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a5f60-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a5f60-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5f60-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a5f60-110">Prerequisites</span></span>

<span data-ttu-id="a5f60-111">Konfigurace integrace Azure AD s Samanage, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a5f60-111">To configure Azure AD integration with Samanage, you need the following items:</span></span>

- <span data-ttu-id="a5f60-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5f60-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a5f60-113">Samanage jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a5f60-113">A Samanage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a5f60-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a5f60-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a5f60-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a5f60-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a5f60-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a5f60-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a5f60-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a5f60-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a5f60-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a5f60-118">Scenario description</span></span>
<span data-ttu-id="a5f60-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a5f60-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a5f60-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a5f60-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a5f60-121">Přidání Samanage z Galerie</span><span class="sxs-lookup"><span data-stu-id="a5f60-121">Adding Samanage from the gallery</span></span>
2. <span data-ttu-id="a5f60-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a5f60-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-samanage-from-the-gallery"></a><span data-ttu-id="a5f60-123">Přidání Samanage z Galerie</span><span class="sxs-lookup"><span data-stu-id="a5f60-123">Adding Samanage from the gallery</span></span>
<span data-ttu-id="a5f60-124">Při konfiguraci integrace Samanage do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Samanage z galerie.</span><span class="sxs-lookup"><span data-stu-id="a5f60-124">To configure the integration of Samanage into Azure AD, you need to add Samanage from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a5f60-125">**Pokud chcete přidat Samanage z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a5f60-125">**To add Samanage from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a5f60-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a5f60-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a5f60-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a5f60-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a5f60-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a5f60-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a5f60-133">Do vyhledávacího pole zadejte **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-133">In the search box, type **Samanage**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_search.png)

5. <span data-ttu-id="a5f60-135">Na panelu výsledků vyberte **Samanage**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a5f60-135">In the results panel, select **Samanage**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a5f60-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a5f60-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a5f60-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Samanage podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a5f60-138">In this section, you configure and test Azure AD single sign-on with Samanage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a5f60-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Samanage je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a5f60-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Samanage is to a user in Azure AD.</span></span> <span data-ttu-id="a5f60-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Samanage musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a5f60-140">In other words, a link relationship between an Azure AD user and the related user in Samanage needs to be established.</span></span>

<span data-ttu-id="a5f60-141">V Samanage, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a5f60-141">In Samanage, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a5f60-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Samanage, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a5f60-142">To configure and test Azure AD single sign-on with Samanage, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a5f60-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a5f60-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a5f60-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a5f60-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a5f60-145">**[Vytvoření zkušebního uživatele Samanage](#creating-a-samanage-test-user)**  – Pokud chcete mít protějšek Britta Simon v Samanage propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5f60-145">**[Creating a Samanage test user](#creating-a-samanage-test-user)** - to have a counterpart of Britta Simon in Samanage that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a5f60-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a5f60-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a5f60-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a5f60-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a5f60-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a5f60-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a5f60-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Samanage.</span><span class="sxs-lookup"><span data-stu-id="a5f60-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Samanage application.</span></span>

<span data-ttu-id="a5f60-150">**Ke konfiguraci Azure AD jednotné přihlašování s Samanage, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a5f60-150">**To configure Azure AD single sign-on with Samanage, perform the following steps:**</span></span>

1. <span data-ttu-id="a5f60-151">Na portálu Azure na **Samanage** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-151">In the Azure portal, on the **Samanage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a5f60-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a5f60-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_samlbase.png)

3. <span data-ttu-id="a5f60-155">Na **Samanage domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a5f60-155">On the **Samanage Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_url.png)

    <span data-ttu-id="a5f60-157">a.</span><span class="sxs-lookup"><span data-stu-id="a5f60-157">a.</span></span> <span data-ttu-id="a5f60-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<Company Name>.samanage.com/saml_login/<Company Name>`</span><span class="sxs-lookup"><span data-stu-id="a5f60-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<Company Name>.samanage.com/saml_login/<Company Name>`</span></span>

    <span data-ttu-id="a5f60-159">b.</span><span class="sxs-lookup"><span data-stu-id="a5f60-159">b.</span></span> <span data-ttu-id="a5f60-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<Company Name>.samanage.com`</span><span class="sxs-lookup"><span data-stu-id="a5f60-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<Company Name>.samanage.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a5f60-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a5f60-161">These values are not real.</span></span> <span data-ttu-id="a5f60-162">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor, který je vysvětlen později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a5f60-162">Update these values with the actual Sign-on URL and Identifier, which is explained later in the tutorial.</span></span> <span data-ttu-id="a5f60-163">Obraťte se na podrobnosti [tým podpory Samanage klienta](https://www.samanage.com/support).</span><span class="sxs-lookup"><span data-stu-id="a5f60-163">For more details contact [Samanage Client support team](https://www.samanage.com/support).</span></span>    
 
4. <span data-ttu-id="a5f60-164">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a5f60-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_certificate.png) 

5. <span data-ttu-id="a5f60-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a5f60-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a5f60-168">Na **Samanage konfigurace** klikněte na tlačítko **konfigurace Samanage** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a5f60-168">On the **Samanage Configuration** section, click **Configure Samanage** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a5f60-169">Kopírování **Sign-Out adresy URL a SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a5f60-169">Copy the **Sign-Out URL, and SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_configure.png) 

7. <span data-ttu-id="a5f60-171">V okně prohlížeče jiný web Přihlaste se jako správce k serveru vaší společnosti Samanage.</span><span class="sxs-lookup"><span data-stu-id="a5f60-171">In a different web browser window, log into your Samanage company site as an administrator.</span></span>

8. <span data-ttu-id="a5f60-172">Klikněte na tlačítko **řídicí panel** a vyberte **instalační program** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="a5f60-172">Click **Dashboard** and select **Setup** in left navigation pane.</span></span>
   
    <span data-ttu-id="a5f60-173">![Řídicí panel](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "řídicí panel")</span><span class="sxs-lookup"><span data-stu-id="a5f60-173">![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")</span></span>

9. <span data-ttu-id="a5f60-174">Klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-174">Click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="a5f60-175">![Jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "jednotného přihlašování")</span><span class="sxs-lookup"><span data-stu-id="a5f60-175">![Single Sign-On](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "Single Sign-On")</span></span>

10. <span data-ttu-id="a5f60-176">Přejděte na **přihlášení pomocí SAML** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a5f60-176">Navigate to **Login using SAML** section, perform the following steps:</span></span>
   
    <span data-ttu-id="a5f60-177">![Přihlášení pomocí SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "přihlášení pomocí SAML")</span><span class="sxs-lookup"><span data-stu-id="a5f60-177">![Login using SAML](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "Login using SAML")</span></span>
 
    <span data-ttu-id="a5f60-178">a.</span><span class="sxs-lookup"><span data-stu-id="a5f60-178">a.</span></span> <span data-ttu-id="a5f60-179">Klikněte na tlačítko **povolit jednotné přihlašování s SAML**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-179">Click **Enable Single Sign-On with SAML**.</span></span>  
 
    <span data-ttu-id="a5f60-180">b.</span><span class="sxs-lookup"><span data-stu-id="a5f60-180">b.</span></span> <span data-ttu-id="a5f60-181">V **adresa URL poskytovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5f60-181">In the **Identity Provider URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>    
 
    <span data-ttu-id="a5f60-182">c.</span><span class="sxs-lookup"><span data-stu-id="a5f60-182">c.</span></span> <span data-ttu-id="a5f60-183">Potvrďte **přihlašovací adresa URL** odpovídá **přihlašovací adresa URL** z **Samanage domény a adresy URL** části na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5f60-183">Confirm the **Login URL** matches the **Sign On URL** of **Samanage Domain and URLs** section in Azure portal.</span></span>
 
    <span data-ttu-id="a5f60-184">d.</span><span class="sxs-lookup"><span data-stu-id="a5f60-184">d.</span></span> <span data-ttu-id="a5f60-185">V **adresy URL odhlašovací** textovému poli, zadejte hodnotu **Sign-Out URL** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a5f60-185">In the **Logout URL** textbox, enter the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="a5f60-186">e.</span><span class="sxs-lookup"><span data-stu-id="a5f60-186">e.</span></span> <span data-ttu-id="a5f60-187">V **SAML vystavitele** textové pole, zadejte id URI aplikace nastavte u poskytovatele identit.</span><span class="sxs-lookup"><span data-stu-id="a5f60-187">In the **SAML Issuer** textbox, type the app id URI set in your identity provider.</span></span>
 
    <span data-ttu-id="a5f60-188">f.</span><span class="sxs-lookup"><span data-stu-id="a5f60-188">f.</span></span> <span data-ttu-id="a5f60-189">Otevření kódovaného certifikátu kódování base-64 stáhli z portálu Azure v programu Poznámkový blok, zkopírujte obsah ho do schránky a vložte jej do **vložit vašeho poskytovatele identit x.509 certifikátu níže** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a5f60-189">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **Paste your Identity Provider x.509 Certificate below** textbox.</span></span>
 
    <span data-ttu-id="a5f60-190">g.</span><span class="sxs-lookup"><span data-stu-id="a5f60-190">g.</span></span> <span data-ttu-id="a5f60-191">Klikněte na tlačítko **vytváření uživatelů, pokud neexistují v Samanage**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-191">Click **Create users if they do not exist in Samanage**.</span></span>
 
    <span data-ttu-id="a5f60-192">h.</span><span class="sxs-lookup"><span data-stu-id="a5f60-192">h.</span></span> <span data-ttu-id="a5f60-193">Klikněte na tlačítko **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-193">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="a5f60-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a5f60-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a5f60-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a5f60-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a5f60-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a5f60-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a5f60-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5f60-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="a5f60-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a5f60-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a5f60-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a5f60-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a5f60-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a5f60-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a5f60-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a5f60-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a5f60-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a5f60-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a5f60-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a5f60-209">a.</span><span class="sxs-lookup"><span data-stu-id="a5f60-209">a.</span></span> <span data-ttu-id="a5f60-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a5f60-211">b.</span><span class="sxs-lookup"><span data-stu-id="a5f60-211">b.</span></span> <span data-ttu-id="a5f60-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a5f60-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a5f60-213">c.</span><span class="sxs-lookup"><span data-stu-id="a5f60-213">c.</span></span> <span data-ttu-id="a5f60-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a5f60-215">d.</span><span class="sxs-lookup"><span data-stu-id="a5f60-215">d.</span></span> <span data-ttu-id="a5f60-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-216">Click **Create**.</span></span>
 
### <a name="creating-a-samanage-test-user"></a><span data-ttu-id="a5f60-217">Vytvoření zkušebního uživatele Samanage</span><span class="sxs-lookup"><span data-stu-id="a5f60-217">Creating a Samanage test user</span></span>

<span data-ttu-id="a5f60-218">Pokud chcete povolit uživatelům Azure AD přihlášení k Samanage, musí být zřízená do Samanage.</span><span class="sxs-lookup"><span data-stu-id="a5f60-218">To enable Azure AD users to log in to Samanage, they must be provisioned into Samanage.</span></span>  
<span data-ttu-id="a5f60-219">V případě Samanage zřizování je ruční úloha.</span><span class="sxs-lookup"><span data-stu-id="a5f60-219">In the case of Samanage, provisioning is a manual task.</span></span>

<span data-ttu-id="a5f60-220">**K poskytnutí uživatelského účtu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a5f60-220">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="a5f60-221">Přihlaste se k serveru vaší společnosti Samanage jako správce.</span><span class="sxs-lookup"><span data-stu-id="a5f60-221">Log into your Samanage company site as an administrator.</span></span>

2. <span data-ttu-id="a5f60-222">Klikněte na tlačítko **řídicí panel** a vyberte **instalační program** v levém navigačním podokně.</span><span class="sxs-lookup"><span data-stu-id="a5f60-222">Click **Dashboard** and select **Setup** in left navigation pan.</span></span>
   
    <span data-ttu-id="a5f60-223">![Instalační program](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "instalační program")</span><span class="sxs-lookup"><span data-stu-id="a5f60-223">![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")</span></span>

3. <span data-ttu-id="a5f60-224">Klikněte **uživatelé** karta</span><span class="sxs-lookup"><span data-stu-id="a5f60-224">Click the **Users** tab</span></span>
   
    <span data-ttu-id="a5f60-225">![Uživatelé](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "uživatelů")</span><span class="sxs-lookup"><span data-stu-id="a5f60-225">![Users](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "Users")</span></span>

4. <span data-ttu-id="a5f60-226">Klikněte na tlačítko **nového uživatele**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-226">Click **New User**.</span></span>
   
    <span data-ttu-id="a5f60-227">![Nový uživatel](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "nového uživatele")</span><span class="sxs-lookup"><span data-stu-id="a5f60-227">![New User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "New User")</span></span>

5. <span data-ttu-id="a5f60-228">Typ **název** a **e-mailovou adresu** účtu Azure Active Directory, které chcete zřídit a klikněte na tlačítko **vytvořit uživateli**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-228">Type the **Name** and the **Email Address** of an Azure Active Directory account you want to provision and click **Create user**.</span></span>
   
    <span data-ttu-id="a5f60-229">![Vytvoření uživatele](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "vytvoření uživatele")</span><span class="sxs-lookup"><span data-stu-id="a5f60-229">![Create User](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "Create User")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="a5f60-230">Držitel účtu Azure Active Directory bude dostávat e-mailu a postupujte podle odkaz potvrďte svůj účet, pak se změní na aktivní.</span><span class="sxs-lookup"><span data-stu-id="a5f60-230">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span> <span data-ttu-id="a5f60-231">Můžete použít všechny ostatní Samanage uživatele účtu nástroje pro tvorbu nebo rozhraní API poskytované Samanage zřídit služby Azure Active Directory uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="a5f60-231">You can use any other Samanage user account creation tools or APIs provided by Samanage to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a5f60-232">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5f60-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a5f60-233">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Samanage.</span><span class="sxs-lookup"><span data-stu-id="a5f60-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Samanage.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a5f60-235">**Pokud chcete přiřadit Britta Simon Samanage, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a5f60-235">**To assign Britta Simon to Samanage, perform the following steps:**</span></span>

1. <span data-ttu-id="a5f60-236">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a5f60-238">V seznamu aplikací vyberte **Samanage**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-238">In the applications list, select **Samanage**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_app.png) 

3. <span data-ttu-id="a5f60-240">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a5f60-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a5f60-242">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a5f60-242">Click **Add** button.</span></span> <span data-ttu-id="a5f60-243">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a5f60-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a5f60-245">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a5f60-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a5f60-246">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a5f60-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a5f60-247">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a5f60-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a5f60-248">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a5f60-248">Testing single sign-on</span></span>

<span data-ttu-id="a5f60-249">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a5f60-249">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a5f60-250">Když kliknete na dlaždici Samanage na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Samanage.</span><span class="sxs-lookup"><span data-stu-id="a5f60-250">When you click the Samanage tile in the Access Panel, you should get automatically signed-on to your Samanage application.</span></span>
<span data-ttu-id="a5f60-251">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a5f60-251">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5f60-252">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a5f60-252">Additional resources</span></span>

* [<span data-ttu-id="a5f60-253">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a5f60-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a5f60-254">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a5f60-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samanage-tutorial/tutorial_general_203.png

