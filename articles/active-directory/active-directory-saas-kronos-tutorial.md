---
title: 'Kurz: Azure Active Directory integrace s Kronos | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Kronos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e28d6191-c375-43c6-b2df-22daa88d9939
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: eb61ec0a7d3e992a285b1af3d4a7fbe1feb8d991
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kronos"></a><span data-ttu-id="497f8-103">Kurz: Azure Active Directory integrace s Kronos</span><span class="sxs-lookup"><span data-stu-id="497f8-103">Tutorial: Azure Active Directory integration with Kronos</span></span>

<span data-ttu-id="497f8-104">V tomto kurzu zjistěte, jak integrovat Kronos s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="497f8-104">In this tutorial, you learn how to integrate Kronos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="497f8-105">Integrace Kronos s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="497f8-105">Integrating Kronos with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="497f8-106">Můžete řídit ve službě Azure AD, který má přístup k Kronos</span><span class="sxs-lookup"><span data-stu-id="497f8-106">You can control in Azure AD who has access to Kronos</span></span>
- <span data-ttu-id="497f8-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Kronos (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="497f8-107">You can enable your users to automatically get signed-on to Kronos (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="497f8-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="497f8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="497f8-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="497f8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="497f8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="497f8-110">Prerequisites</span></span>

<span data-ttu-id="497f8-111">Konfigurace integrace Azure AD s Kronos, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="497f8-111">To configure Azure AD integration with Kronos, you need the following items:</span></span>

- <span data-ttu-id="497f8-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="497f8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="497f8-113">A **Kronos pracovníků centrální** předplatné povolené jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="497f8-113">A **Kronos Workforce Central** SSO enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="497f8-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="497f8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="497f8-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="497f8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="497f8-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="497f8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="497f8-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="497f8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="497f8-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="497f8-118">Scenario description</span></span>
<span data-ttu-id="497f8-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="497f8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="497f8-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="497f8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="497f8-121">Přidání Kronos z Galerie</span><span class="sxs-lookup"><span data-stu-id="497f8-121">Adding Kronos from the gallery</span></span>
2. <span data-ttu-id="497f8-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="497f8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kronos-from-the-gallery"></a><span data-ttu-id="497f8-123">Přidání Kronos z Galerie</span><span class="sxs-lookup"><span data-stu-id="497f8-123">Adding Kronos from the gallery</span></span>
<span data-ttu-id="497f8-124">Při konfiguraci integrace Kronos do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS Kronos z galerie.</span><span class="sxs-lookup"><span data-stu-id="497f8-124">To configure the integration of Kronos into Azure AD, you need to add Kronos from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="497f8-125">**Pokud chcete přidat Kronos z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="497f8-125">**To add Kronos from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="497f8-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="497f8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="497f8-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="497f8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="497f8-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="497f8-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="497f8-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="497f8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="497f8-133">Do vyhledávacího pole zadejte **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="497f8-133">In the search box, type **Kronos**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_search.png)

5. <span data-ttu-id="497f8-135">Na panelu výsledků vyberte **Kronos**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="497f8-135">In the results panel, select **Kronos**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="497f8-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="497f8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="497f8-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kronos podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="497f8-138">In this section, you configure and test Azure AD single sign-on with Kronos based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="497f8-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Kronos je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="497f8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kronos is to a user in Azure AD.</span></span> <span data-ttu-id="497f8-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v Kronos musí navázat.</span><span class="sxs-lookup"><span data-stu-id="497f8-140">In other words, a link relationship between an Azure AD user and the related user in Kronos needs to be established.</span></span>

<span data-ttu-id="497f8-141">V Kronos, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="497f8-141">In Kronos, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="497f8-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Kronos, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="497f8-142">To configure and test Azure AD single sign-on with Kronos, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="497f8-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="497f8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="497f8-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="497f8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="497f8-145">**[Vytvoření zkušebního uživatele Kronos](#creating-a-kronos-test-user)**  – Pokud chcete mít protějšek Britta Simon v Kronos propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="497f8-145">**[Creating a Kronos test user](#creating-a-kronos-test-user)** - to have a counterpart of Britta Simon in Kronos that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="497f8-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="497f8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="497f8-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="497f8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="497f8-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="497f8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="497f8-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Kronos.</span><span class="sxs-lookup"><span data-stu-id="497f8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kronos application.</span></span>

<span data-ttu-id="497f8-150">**Ke konfiguraci Azure AD jednotné přihlašování s Kronos, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="497f8-150">**To configure Azure AD single sign-on with Kronos, perform the following steps:**</span></span>

1. <span data-ttu-id="497f8-151">Na portálu Azure na **Kronos** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="497f8-151">In the Azure portal, on the **Kronos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="497f8-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="497f8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_samlbase.png)

3. <span data-ttu-id="497f8-155">Na **Kronos domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="497f8-155">On the **Kronos Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_url.png)

    <span data-ttu-id="497f8-157">a.</span><span class="sxs-lookup"><span data-stu-id="497f8-157">a.</span></span> <span data-ttu-id="497f8-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.kronos.net/`</span><span class="sxs-lookup"><span data-stu-id="497f8-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.kronos.net/`</span></span>

    <span data-ttu-id="497f8-159">b.</span><span class="sxs-lookup"><span data-stu-id="497f8-159">b.</span></span> <span data-ttu-id="497f8-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span><span class="sxs-lookup"><span data-stu-id="497f8-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="497f8-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="497f8-161">These values are not real.</span></span> <span data-ttu-id="497f8-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="497f8-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="497f8-163">Obraťte se na [tým podpory Kronos](https://www.kronos.in/contact/en-in/form) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="497f8-163">Contact [Kronos support team](https://www.kronos.in/contact/en-in/form) to get these values.</span></span>
 
4. <span data-ttu-id="497f8-164">Aplikace Kronos očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="497f8-164">Your Kronos application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="497f8-165">Práce s [tým podpory Kronos](https://www.kronos.in/contact/en-in/form) nejprve k identifikaci správné uživatelský identifikátor, který je namapován do aplikace.</span><span class="sxs-lookup"><span data-stu-id="497f8-165">Work with [Kronos support team](https://www.kronos.in/contact/en-in/form) first to identify the correct user identifier, which is mapped into the application.</span></span> <span data-ttu-id="497f8-166">Proveďte taky pokyny pro atribut, který se má použít pro toto mapování.</span><span class="sxs-lookup"><span data-stu-id="497f8-166">Also please take the guidance about the attribute, which they want to use for this mapping.</span></span>
 
     <span data-ttu-id="497f8-167">Společnost Microsoft doporučuje používat **"NameIdentifier"** atribut jako identifikátor uživatele.</span><span class="sxs-lookup"><span data-stu-id="497f8-167">Microsoft recommends using the **"NameIdentifier"** attribute as user identifier.</span></span> <span data-ttu-id="497f8-168">Můžete spravovat hodnoty těchto atributů z **"Atributy uživatele"** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="497f8-168">You can manage the values of these attributes from the **"User Attributes"** section on application integration page.</span></span>
     
     <span data-ttu-id="497f8-169">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="497f8-169">The following screenshot shows an example for this.</span></span> <span data-ttu-id="497f8-170">Zde jsme jsou namapovány **uživatelský identifikátor (nameid)** s **ExtractMailPrefix()** funkce **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="497f8-170">Here we have mapped the **User Identifier (nameid)** with **ExtractMailPrefix()** function of **user.userprincipalname**.</span></span> <span data-ttu-id="497f8-171">Díky tomu hodnotu předpony e-mailů uživatele, který je jedinečné ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="497f8-171">This gives the prefix value of email of the user which is the unique User ID.</span></span> <span data-ttu-id="497f8-172">Ta se odešle do aplikace Kronos v každé úspěšné odpovědi.</span><span class="sxs-lookup"><span data-stu-id="497f8-172">This is sent to the Kronos application in every successful response.</span></span> 
     
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_attribute.png)

5. <span data-ttu-id="497f8-174">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="497f8-174">In the **User Attributes** section on the **Single sign-on** dialog:</span></span>

    <span data-ttu-id="497f8-175">a.</span><span class="sxs-lookup"><span data-stu-id="497f8-175">a.</span></span> <span data-ttu-id="497f8-176">V rozevíracím seznamu identifikátor uživatele, vyberte **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="497f8-176">In the User Identifier dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="497f8-177">b.</span><span class="sxs-lookup"><span data-stu-id="497f8-177">b.</span></span> <span data-ttu-id="497f8-178">V **e-mailu** rozevíracího seznamu vyberte **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="497f8-178">In the **Mail** dropdown list, select **user.userprincipalname**.</span></span>

6. <span data-ttu-id="497f8-179">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="497f8-179">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_certificate.png) 

7. <span data-ttu-id="497f8-181">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="497f8-181">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="497f8-183">Konfigurace jednotného přihlašování na **Kronos** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory Kronos](https://www.kronos.in/contact/en-in/form).</span><span class="sxs-lookup"><span data-stu-id="497f8-183">To configure single sign-on on **Kronos** side, you need to send the downloaded **Metadata XML** to [Kronos support team](https://www.kronos.in/contact/en-in/form).</span></span> 

> [!TIP]
> <span data-ttu-id="497f8-184">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="497f8-184">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="497f8-185">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="497f8-185">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="497f8-186">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="497f8-186">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="497f8-187">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="497f8-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="497f8-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="497f8-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="497f8-190">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="497f8-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="497f8-191">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="497f8-191">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="497f8-193">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="497f8-193">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="497f8-195">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="497f8-195">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="497f8-197">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="497f8-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="497f8-199">a.</span><span class="sxs-lookup"><span data-stu-id="497f8-199">a.</span></span> <span data-ttu-id="497f8-200">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="497f8-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="497f8-201">b.</span><span class="sxs-lookup"><span data-stu-id="497f8-201">b.</span></span> <span data-ttu-id="497f8-202">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="497f8-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="497f8-203">c.</span><span class="sxs-lookup"><span data-stu-id="497f8-203">c.</span></span> <span data-ttu-id="497f8-204">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="497f8-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="497f8-205">d.</span><span class="sxs-lookup"><span data-stu-id="497f8-205">d.</span></span> <span data-ttu-id="497f8-206">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="497f8-206">Click **Create**.</span></span>
 
### <a name="creating-a-kronos-test-user"></a><span data-ttu-id="497f8-207">Vytvoření zkušebního uživatele Kronos</span><span class="sxs-lookup"><span data-stu-id="497f8-207">Creating a Kronos test user</span></span>

<span data-ttu-id="497f8-208">V této části vytvoříte volal Britta Simon v Kronos uživatele.</span><span class="sxs-lookup"><span data-stu-id="497f8-208">In this section, you create a user called Britta Simon in Kronos.</span></span> <span data-ttu-id="497f8-209">Kronos aplikace, musí všichni uživatelé zřídit v aplikaci před provedením jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="497f8-209">Kronos application needs all the users to be provisioned in the application before doing SSO.</span></span> 

<span data-ttu-id="497f8-210">Pracovat [tým podpory Kronos](https://www.kronos.in/contact/en-in/form) ke zřízení tito uživatelé do aplikace.</span><span class="sxs-lookup"><span data-stu-id="497f8-210">Work with the [Kronos support team](https://www.kronos.in/contact/en-in/form) to provision all these users into the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="497f8-211">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="497f8-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="497f8-212">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu Kronos.</span><span class="sxs-lookup"><span data-stu-id="497f8-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kronos.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="497f8-214">**Pokud chcete přiřadit Britta Simon Kronos, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="497f8-214">**To assign Britta Simon to Kronos, perform the following steps:**</span></span>

1. <span data-ttu-id="497f8-215">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="497f8-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="497f8-217">V seznamu aplikací vyberte **Kronos**.</span><span class="sxs-lookup"><span data-stu-id="497f8-217">In the applications list, select **Kronos**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_app.png) 

3. <span data-ttu-id="497f8-219">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="497f8-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="497f8-221">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="497f8-221">Click **Add** button.</span></span> <span data-ttu-id="497f8-222">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="497f8-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="497f8-224">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="497f8-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="497f8-225">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="497f8-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="497f8-226">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="497f8-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="497f8-227">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="497f8-227">Testing single sign-on</span></span>

<span data-ttu-id="497f8-228">V této části můžete otestovat vaši konfiguraci Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="497f8-228">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="497f8-229">Když kliknete na dlaždici Kronos na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Kronos.</span><span class="sxs-lookup"><span data-stu-id="497f8-229">When you click the Kronos tile in the Access Panel, you should get automatically signed-on to your Kronos application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="497f8-230">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="497f8-230">Additional resources</span></span>

* [<span data-ttu-id="497f8-231">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="497f8-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="497f8-232">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="497f8-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png

