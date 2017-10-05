---
title: 'Kurz: Azure Active Directory integrace s Hackerone | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Hackerone."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 229d1efb-b6a5-4df8-9839-5d551487db4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 657d8d4c98b7b133698a5cda0aa675da7f68c464
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a><span data-ttu-id="6e2d9-103">Kurz: Azure Active Directory integrace s HackerOne</span><span class="sxs-lookup"><span data-stu-id="6e2d9-103">Tutorial: Azure Active Directory integration with HackerOne</span></span>

<span data-ttu-id="6e2d9-104">V tomto kurzu zjistěte, jak integrovat HackerOne s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6e2d9-104">In this tutorial, you learn how to integrate HackerOne with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6e2d9-105">Integrace HackerOne s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-105">Integrating HackerOne with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6e2d9-106">Můžete řídit ve službě Azure AD, který má přístup k HackerOne</span><span class="sxs-lookup"><span data-stu-id="6e2d9-106">You can control in Azure AD who has access to HackerOne</span></span>
- <span data-ttu-id="6e2d9-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k HackerOne (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e2d9-107">You can enable your users to automatically get signed-on to HackerOne (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6e2d9-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="6e2d9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6e2d9-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6e2d9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e2d9-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6e2d9-110">Prerequisites</span></span>

<span data-ttu-id="6e2d9-111">Konfigurace integrace Azure AD s HackerOne, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-111">To configure Azure AD integration with HackerOne, you need the following items:</span></span>

- <span data-ttu-id="6e2d9-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e2d9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6e2d9-113">HackerOne jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="6e2d9-113">A HackerOne single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6e2d9-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6e2d9-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6e2d9-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6e2d9-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6e2d9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6e2d9-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="6e2d9-118">Scenario description</span></span>
<span data-ttu-id="6e2d9-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6e2d9-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6e2d9-121">Přidání HackerOne z Galerie</span><span class="sxs-lookup"><span data-stu-id="6e2d9-121">Adding HackerOne from the gallery</span></span>
2. <span data-ttu-id="6e2d9-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e2d9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hackerone-from-the-gallery"></a><span data-ttu-id="6e2d9-123">Přidání HackerOne z Galerie</span><span class="sxs-lookup"><span data-stu-id="6e2d9-123">Adding HackerOne from the gallery</span></span>
<span data-ttu-id="6e2d9-124">Při konfiguraci integrace HackerOne do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS HackerOne z galerie.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-124">To configure the integration of HackerOne into Azure AD, you need to add HackerOne from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6e2d9-125">**Pokud chcete přidat HackerOne z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6e2d9-125">**To add HackerOne from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6e2d9-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6e2d9-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6e2d9-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="6e2d9-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="6e2d9-133">Do vyhledávacího pole zadejte **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-133">In the search box, type **HackerOne**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_search.png)

5. <span data-ttu-id="6e2d9-135">Na panelu výsledků vyberte **HackerOne**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-135">In the results panel, select **HackerOne**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6e2d9-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e2d9-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="6e2d9-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s HackerOne podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="6e2d9-138">In this section, you configure and test Azure AD single sign-on with HackerOne based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6e2d9-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v HackerOne je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HackerOne is to a user in Azure AD.</span></span> <span data-ttu-id="6e2d9-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v HackerOne musí navázat.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-140">In other words, a link relationship between an Azure AD user and the related user in HackerOne needs to be established.</span></span>

<span data-ttu-id="6e2d9-141">V HackerOne, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-141">In HackerOne, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6e2d9-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s HackerOne, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-142">To configure and test Azure AD single sign-on with HackerOne, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6e2d9-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6e2d9-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6e2d9-145">**[Vytvoření zkušebního uživatele HackerOne](#creating-a-hackerone-test-user)**  – Pokud chcete mít protějšek Britta Simon v HackerOne propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-145">**[Creating a HackerOne test user](#creating-a-hackerone-test-user)** - to have a counterpart of Britta Simon in HackerOne that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6e2d9-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6e2d9-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6e2d9-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e2d9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6e2d9-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci HackerOne.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HackerOne application.</span></span>

<span data-ttu-id="6e2d9-150">**Ke konfiguraci Azure AD jednotné přihlašování s HackerOne, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6e2d9-150">**To configure Azure AD single sign-on with HackerOne, perform the following steps:**</span></span>

1. <span data-ttu-id="6e2d9-151">Na portálu Azure na **HackerOne** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-151">In the Azure portal, on the **HackerOne** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="6e2d9-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_samlbase.png)

3. <span data-ttu-id="6e2d9-155">Na **HackerOne jedním přihlašovací adresa URL a identifikátor** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-155">On the **HackerOne Single sign-on URL and Identifier** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_url.png)

    <span data-ttu-id="6e2d9-157">a.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-157">a.</span></span> <span data-ttu-id="6e2d9-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://hackerone.com/<company name>/authentication`</span><span class="sxs-lookup"><span data-stu-id="6e2d9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://hackerone.com/<company name>/authentication`</span></span>

    <span data-ttu-id="6e2d9-159">b.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-159">b.</span></span> <span data-ttu-id="6e2d9-160">V **identifikátor** textovému poli, zadejte adresu URL jako:`https://hackerone.com/users/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="6e2d9-160">In the **Identifier** textbox, type a URL as:  `https://hackerone.com/users/saml/metadata`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="6e2d9-161">Tato hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-161">This value is not real.</span></span> <span data-ttu-id="6e2d9-162">Aktualizujte tuto hodnotu s skutečná adresa URL přihlašování.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="6e2d9-163">Obraťte se na [tým podpory HackerOne](mailto:support@hackerone.com) získat tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-163">Contact [HackerOne support team](mailto:support@hackerone.com) to get this value.</span></span> 
 
4. <span data-ttu-id="6e2d9-164">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_certificate.png) 

5. <span data-ttu-id="6e2d9-166">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-166">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6e2d9-168">Na **HackerOne konfigurace** klikněte na tlačítko **konfigurace HackerOne** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-168">On the **HackerOne Configuration** section, click **Configure HackerOne** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6e2d9-169">Kopírování **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="6e2d9-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_configure.png) 

7. <span data-ttu-id="6e2d9-171">Přihlašování ke klientovi HackerOne jako správce.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-171">Sign On to your HackerOne tenant as an administrator.</span></span>

8. <span data-ttu-id="6e2d9-172">V nabídce v horní části, klikněte "**nastavení**."</span><span class="sxs-lookup"><span data-stu-id="6e2d9-172">In the menu on the top, click the "**Settings**."</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

9. <span data-ttu-id="6e2d9-174">Přejděte na "**ověřování**"a klikněte na tlačítko"**přidejte nastavení SAML**."</span><span class="sxs-lookup"><span data-stu-id="6e2d9-174">Navigate to "**Authentication**" and click "**Add SAML settings**."</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 

10. <span data-ttu-id="6e2d9-176">Na **SAML nastavení** dialogové okno, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-176">On the **SAML Settings** dialog, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    <span data-ttu-id="6e2d9-178">a.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-178">a.</span></span> <span data-ttu-id="6e2d9-179">V **e-mailovou doménu** textovému poli, zadejte registrované domény.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-179">In the **Email Domain** textbox, type a registered domain.</span></span>

    <span data-ttu-id="6e2d9-180">b.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-180">b.</span></span> <span data-ttu-id="6e2d9-181">V **jeden přihlašovací adresa URL** textových polí, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-181">In  **Single Sign On URL** textboxes, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6e2d9-182">c.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-182">c.</span></span> <span data-ttu-id="6e2d9-183">Otevřete váš **soubor certifikátu** v poznámkovém bloku stáhli z portálu Azure, kopírovat obsah ho do schránky a vložte jej do **X509 certifikátu** textové pole.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-183">Open your **Certificate file** in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X509 Certificate**  textbox.</span></span>
    
    <span data-ttu-id="6e2d9-184">d.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-184">d.</span></span> <span data-ttu-id="6e2d9-185">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-185">Click **Save**.</span></span>

11. <span data-ttu-id="6e2d9-186">V dialogovém okně Nastavení ověřování proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-186">On the Authentication Settings dialog, perform the following steps:</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    <span data-ttu-id="6e2d9-188">a.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-188">a.</span></span> <span data-ttu-id="6e2d9-189">Klikněte na tlačítko **spuštění testu**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-189">Click **Run test**.</span></span>

    <span data-ttu-id="6e2d9-190">b.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-190">b.</span></span> <span data-ttu-id="6e2d9-191">Pokud hodnota **stav** pole rovná **poslední stav testu: vytvořili**, obraťte vaše [tým podpory HackerOne](mailto:support@hackerone.com) k žádosti o konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-191">If the value of the **Status** field equals **Last test status: created**, contact your [HackerOne support team](mailto:support@hackerone.com) to request a review of your configuration.</span></span>

> [!TIP]
> <span data-ttu-id="6e2d9-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="6e2d9-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6e2d9-193">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6e2d9-194">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6e2d9-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6e2d9-195">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e2d9-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="6e2d9-196">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="6e2d9-198">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6e2d9-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6e2d9-199">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6e2d9-201">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6e2d9-203">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6e2d9-205">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6e2d9-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6e2d9-207">a.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-207">a.</span></span> <span data-ttu-id="6e2d9-208">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6e2d9-209">b.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-209">b.</span></span> <span data-ttu-id="6e2d9-210">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6e2d9-211">c.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-211">c.</span></span> <span data-ttu-id="6e2d9-212">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6e2d9-213">d.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-213">d.</span></span> <span data-ttu-id="6e2d9-214">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-214">Click **Create**.</span></span>
 
### <a name="creating-a-hackerone-test-user"></a><span data-ttu-id="6e2d9-215">Vytvoření zkušebního uživatele HackerOne</span><span class="sxs-lookup"><span data-stu-id="6e2d9-215">Creating a HackerOne test user</span></span>

<span data-ttu-id="6e2d9-216">Potom vytvořte uživatele v HackerOne nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-216">Next, you create a user called Britta Simon in HackerOne.</span></span> <span data-ttu-id="6e2d9-217">HackerOne podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-217">HackerOne supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="6e2d9-218">Neexistuje žádná položka akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-218">There is no action item for you in this section.</span></span> <span data-ttu-id="6e2d9-219">Při přístupu k HackerOne, je vytvořit nového uživatele, pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-219">When you access HackerOne, a new user is created if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="6e2d9-220">Pokud je potřeba ručně vytvořit uživateli, budete muset kontaktujte tým podpory Certify.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-220">If you need to create a user manually, you need to contact the Certify support team.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6e2d9-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="6e2d9-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6e2d9-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu HackerOne.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HackerOne.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="6e2d9-224">**Pokud chcete přiřadit Britta Simon HackerOne, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="6e2d9-224">**To assign Britta Simon to HackerOne, perform the following steps:**</span></span>

1. <span data-ttu-id="6e2d9-225">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="6e2d9-227">V seznamu aplikací vyberte **HackerOne**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-227">In the applications list, select **HackerOne**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_app.png) 

3. <span data-ttu-id="6e2d9-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="6e2d9-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-231">Click **Add** button.</span></span> <span data-ttu-id="6e2d9-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="6e2d9-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6e2d9-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6e2d9-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6e2d9-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="6e2d9-237">Testing single sign-on</span></span>

<span data-ttu-id="6e2d9-238">Nakonec můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-238">Finally, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="6e2d9-239">Když kliknete na dlaždici HackerOne na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci HackerOne.</span><span class="sxs-lookup"><span data-stu-id="6e2d9-239">When you click the HackerOne tile in the Access Panel, you should get automatically signed-on to your HackerOne application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6e2d9-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6e2d9-240">Additional resources</span></span>

* [<span data-ttu-id="6e2d9-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6e2d9-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6e2d9-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6e2d9-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png

