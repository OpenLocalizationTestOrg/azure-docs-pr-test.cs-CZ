---
title: 'Kurz: Azure Active Directory integrace s Lifesize cloudu | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Lifesize cloudu."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 7542360f9c75786bf400553090ba0a891d9c2fcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a><span data-ttu-id="a732f-103">Kurz: Azure Active Directory integrace s Lifesize cloudu</span><span class="sxs-lookup"><span data-stu-id="a732f-103">Tutorial: Azure Active Directory integration with Lifesize Cloud</span></span>

<span data-ttu-id="a732f-104">V tomto kurzu zjistěte, jak integrovat Lifesize cloudu s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a732f-104">In this tutorial, you learn how to integrate Lifesize Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a732f-105">Integrace Lifesize cloudu s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a732f-105">Integrating Lifesize Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a732f-106">Můžete řídit ve službě Azure AD, který má přístup do cloudu Lifesize</span><span class="sxs-lookup"><span data-stu-id="a732f-106">You can control in Azure AD who has access to Lifesize Cloud</span></span>
- <span data-ttu-id="a732f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k Lifesize cloudu (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a732f-107">You can enable your users to automatically get signed-on to Lifesize Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a732f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a732f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a732f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a732f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a732f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a732f-110">Prerequisites</span></span>

<span data-ttu-id="a732f-111">Ke konfiguraci integrace služby Azure AD s cloudem Lifesize, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a732f-111">To configure Azure AD integration with Lifesize Cloud, you need the following items:</span></span>

- <span data-ttu-id="a732f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a732f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a732f-113">Cloudu Lifesize jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a732f-113">A Lifesize Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a732f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a732f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a732f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a732f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a732f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a732f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a732f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a732f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a732f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a732f-118">Scenario description</span></span>
<span data-ttu-id="a732f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a732f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a732f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a732f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a732f-121">Přidání Lifesize cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="a732f-121">Adding Lifesize Cloud from the gallery</span></span>
2. <span data-ttu-id="a732f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a732f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lifesize-cloud-from-the-gallery"></a><span data-ttu-id="a732f-123">Přidání Lifesize cloudu z Galerie</span><span class="sxs-lookup"><span data-stu-id="a732f-123">Adding Lifesize Cloud from the gallery</span></span>
<span data-ttu-id="a732f-124">Při konfiguraci integrace Lifesize cloudu do služby Azure AD, potřebujete přidat Lifesize cloudu z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="a732f-124">To configure the integration of Lifesize Cloud into Azure AD, you need to add Lifesize Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a732f-125">**Pokud chcete přidat Lifesize cloudu z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a732f-125">**To add Lifesize Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a732f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a732f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a732f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a732f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a732f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a732f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a732f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a732f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a732f-133">Do vyhledávacího pole zadejte **Lifesize cloudu**.</span><span class="sxs-lookup"><span data-stu-id="a732f-133">In the search box, type **Lifesize Cloud**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. <span data-ttu-id="a732f-135">Na panelu výsledků vyberte **Lifesize cloudu**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a732f-135">In the results panel, select **Lifesize Cloud**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a732f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a732f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a732f-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s cloudem Lifesize podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="a732f-138">In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a732f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v cloudu Lifesize je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a732f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lifesize Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="a732f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v cloudu Lifesize musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a732f-140">In other words, a link relationship between an Azure AD user and the related user in Lifesize Cloud needs to be established.</span></span>

<span data-ttu-id="a732f-141">V cloudu Lifesize přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a732f-141">In Lifesize Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a732f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Lifesize cloudu, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a732f-142">To configure and test Azure AD single sign-on with Lifesize Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a732f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a732f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a732f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a732f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a732f-145">**[Vytvoření zkušebního uživatele Lifesize Cloud](#creating-a-lifesize-cloud-test-user)**  – Pokud chcete mít protějšek Britta Simon Lifesize cloudu, který je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a732f-145">**[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - to have a counterpart of Britta Simon in Lifesize Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a732f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a732f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a732f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a732f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a732f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a732f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a732f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Lifesize cloudu.</span><span class="sxs-lookup"><span data-stu-id="a732f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lifesize Cloud application.</span></span>

<span data-ttu-id="a732f-150">**Ke konfiguraci Azure AD jednotné přihlašování s Lifesize cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a732f-150">**To configure Azure AD single sign-on with Lifesize Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a732f-151">Na portálu Azure na **Lifesize cloudu** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a732f-151">In the Azure portal, on the **Lifesize Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a732f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a732f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. <span data-ttu-id="a732f-155">Na **Lifesize cloudové domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a732f-155">On the **Lifesize Cloud Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    <span data-ttu-id="a732f-157">a.</span><span class="sxs-lookup"><span data-stu-id="a732f-157">a.</span></span> <span data-ttu-id="a732f-158">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://login.lifesizecloud.com/ls/?acs`</span><span class="sxs-lookup"><span data-stu-id="a732f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/ls/?acs`</span></span>

    <span data-ttu-id="a732f-159">b.</span><span class="sxs-lookup"><span data-stu-id="a732f-159">b.</span></span> <span data-ttu-id="a732f-160">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://login.lifesizecloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="a732f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/<companyname>`</span></span>

     
4. <span data-ttu-id="a732f-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="a732f-161">Check **Show advanced URL settings**, perform the following step:</span></span>    
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    <span data-ttu-id="a732f-163">V **předávání stavu** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://webapp.lifesizecloud.com/?ent=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="a732f-163">In the **Relay state** textbox, type a URL using the following pattern: `https://webapp.lifesizecloud.com/?ent=<identifier>`</span></span>
   
   > [!NOTE] 
   ><span data-ttu-id="a732f-164">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a732f-164">Please note that these are not the real values.</span></span> <span data-ttu-id="a732f-165">budete muset tyto hodnoty aktualizovat skutečné přihlašovací adresa URL, stav předávání a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="a732f-165">you have to update these values with the actual Sign-On URL, Relay State, and Identifier.</span></span> <span data-ttu-id="a732f-166">Obraťte se na [tým podpory klient Cloud Lifesize](https://www.lifesize.com/support) získat přihlašovací adresa URL a identifikátor hodnoty a můžete získat hodnotu předávání stavu z konfigurace jednotného přihlašování, která je vysvětlená později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a732f-166">Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) to get Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in the tutorial.</span></span>

4. <span data-ttu-id="a732f-167">Na **SAML podpisový certifikát** klikněte na tlačítko **Certificate(Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="a732f-167">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. <span data-ttu-id="a732f-169">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a732f-169">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a732f-171">Na **konfigurace cloudu Lifesize** klikněte na tlačítko **konfigurace cloudu Lifesize** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a732f-171">On the **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a732f-172">Kopírování **SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a732f-172">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. <span data-ttu-id="a732f-174">Chcete-li získat jednotné přihlašování, které jsou nakonfigurované pro vaši aplikaci, přihlášení do aplikace Lifesize cloudu s oprávněními správce.</span><span class="sxs-lookup"><span data-stu-id="a732f-174">To get SSO configured for your application, login into the Lifesize Cloud application with Admin privileges.</span></span>

8. <span data-ttu-id="a732f-175">V pravém horním rohu klikněte na název a potom klikněte na **rozšířená nastavení**.</span><span class="sxs-lookup"><span data-stu-id="a732f-175">In the top right corner click on your name and then click on the **Advance Settings**.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. <span data-ttu-id="a732f-177">V rozšířená nastavení nyní klikněte na **Konfigurace jednotného přihlašování k** odkaz.</span><span class="sxs-lookup"><span data-stu-id="a732f-177">In the Advance Settings now click on the **SSO Configuration** link.</span></span> <span data-ttu-id="a732f-178">Otevře se stránka Konfigurace jednotného přihlašování pro vaše instance.</span><span class="sxs-lookup"><span data-stu-id="a732f-178">It will open the SSO Configuration page for your instance.</span></span>
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. <span data-ttu-id="a732f-180">Teď můžete nakonfigurujte následující hodnoty v uživatelském rozhraní Konfigurace jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a732f-180">Now configure the following values in the SSO configuration UI.</span></span>    
   
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    <span data-ttu-id="a732f-182">a.</span><span class="sxs-lookup"><span data-stu-id="a732f-182">a.</span></span> <span data-ttu-id="a732f-183">V **vystavitele zprostředkovatele Identity** textovému poli, vložte hodnotu **SAML Entity ID** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a732f-183">In **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a732f-184">b.</span><span class="sxs-lookup"><span data-stu-id="a732f-184">b.</span></span>  <span data-ttu-id="a732f-185">V **přihlašovací adresa URL** textovému poli, vložte hodnotu **SAML jeden přihlašování adresa URL služby** který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="a732f-185">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a732f-186">c.</span><span class="sxs-lookup"><span data-stu-id="a732f-186">c.</span></span> <span data-ttu-id="a732f-187">Otevření kódovaného certifikátu kódování base-64 v poznámkovém bloku stáhli z portálu Azure, zkopírujte obsah ho do schránky a vložte jej do **certifikát X.509** textové pole.</span><span class="sxs-lookup"><span data-stu-id="a732f-187">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="a732f-188">d.</span><span class="sxs-lookup"><span data-stu-id="a732f-188">d.</span></span> <span data-ttu-id="a732f-189">V atributu SAML mapování pro název první textového pole zadejte hodnotu jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span><span class="sxs-lookup"><span data-stu-id="a732f-189">In the SAML Attribute mappings for the First Name text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span></span>
    
    <span data-ttu-id="a732f-190">e.</span><span class="sxs-lookup"><span data-stu-id="a732f-190">e.</span></span> <span data-ttu-id="a732f-191">V mapování atributů SAML pro **příjmení** textového pole zadejte hodnotu jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span><span class="sxs-lookup"><span data-stu-id="a732f-191">In the SAML Attribute mapping for the **Last Name** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span></span>
    
    <span data-ttu-id="a732f-192">f.</span><span class="sxs-lookup"><span data-stu-id="a732f-192">f.</span></span> <span data-ttu-id="a732f-193">V mapování atributů SAML pro **e-mailu** textového pole zadejte hodnotu jako **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span><span class="sxs-lookup"><span data-stu-id="a732f-193">In the SAML Attribute mapping for the **Email** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span></span>

11. <span data-ttu-id="a732f-194">Ke kontrole konfigurace můžete kliknutím na **Test** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a732f-194">To check the configuration you can click on the **Test** button.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="a732f-195">Pro úspěšné testování potřebujete k dokončení Průvodce konfigurací služby ve službě Azure AD a také poskytují přístup na uživatele nebo skupiny, které můžete provést test.</span><span class="sxs-lookup"><span data-stu-id="a732f-195">For successful testing you need to complete the configuration wizard in Azure AD and also provide access to users or groups who can perform the test.</span></span>

12. <span data-ttu-id="a732f-196">Povolit jednotné přihlašování kontrolou na **povolit jednotné přihlašování** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a732f-196">Enable the SSO by checking on the **Enable SSO** button.</span></span>

13. <span data-ttu-id="a732f-197">Nyní klikněte na **aktualizace** tlačítko tak, aby se ukládají všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="a732f-197">Now click on the **Update** button so that all the settings are saved.</span></span> <span data-ttu-id="a732f-198">Tím se vygeneruje RelayState hodnota.</span><span class="sxs-lookup"><span data-stu-id="a732f-198">This will generate the RelayState value.</span></span> <span data-ttu-id="a732f-199">Kopírování RelayState hodnotu, která se generují do textového pole, vložte jej do **předávání stavu** textového pole pod **Lifesize cloudové domény a adresy URL** části.</span><span class="sxs-lookup"><span data-stu-id="a732f-199">Copy the RelayState value, which is generated in the text box, paste it in the **Relay State** textbox under **Lifesize Cloud Domain and URLs** section.</span></span> 

> [!TIP]
> <span data-ttu-id="a732f-200">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a732f-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a732f-201">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a732f-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a732f-202">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a732f-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a732f-203">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a732f-203">Creating an Azure AD test user</span></span>

<span data-ttu-id="a732f-204">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a732f-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a732f-206">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a732f-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a732f-207">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a732f-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a732f-209">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a732f-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a732f-211">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a732f-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a732f-213">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a732f-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a732f-215">a.</span><span class="sxs-lookup"><span data-stu-id="a732f-215">a.</span></span> <span data-ttu-id="a732f-216">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a732f-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a732f-217">b.</span><span class="sxs-lookup"><span data-stu-id="a732f-217">b.</span></span> <span data-ttu-id="a732f-218">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a732f-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a732f-219">c.</span><span class="sxs-lookup"><span data-stu-id="a732f-219">c.</span></span> <span data-ttu-id="a732f-220">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a732f-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a732f-221">d.</span><span class="sxs-lookup"><span data-stu-id="a732f-221">d.</span></span> <span data-ttu-id="a732f-222">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a732f-222">Click **Create**.</span></span>
 
### <a name="creating-a-lifesize-cloud-test-user"></a><span data-ttu-id="a732f-223">Vytvoření zkušebního uživatele Lifesize cloudu</span><span class="sxs-lookup"><span data-stu-id="a732f-223">Creating a Lifesize Cloud test user</span></span>

<span data-ttu-id="a732f-224">V této části vytvoříte uživatele názvem Britta Simon v Lifesize cloudu.</span><span class="sxs-lookup"><span data-stu-id="a732f-224">In this section, you create a user called Britta Simon in Lifesize Cloud.</span></span> <span data-ttu-id="a732f-225">Lifesize cloudu podporovat zřizování automatické uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a732f-225">Lifesize cloud does support automatic user provisioning.</span></span> <span data-ttu-id="a732f-226">Po úspěšném ověření v Azure AD se automaticky zřídí uživatele v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a732f-226">After successful authentication at Azure AD, the user will be automatically provisioned in the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a732f-227">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a732f-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a732f-228">V této části povolíte Britta Simon používat tak, že udělíte přístup do cloudu Lifesize Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a732f-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lifesize Cloud.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a732f-230">**Přiřadit Britta Simon Lifesize cloudu, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a732f-230">**To assign Britta Simon to Lifesize Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a732f-231">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a732f-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a732f-233">V seznamu aplikací vyberte **Lifesize cloudu**.</span><span class="sxs-lookup"><span data-stu-id="a732f-233">In the applications list, select **Lifesize Cloud**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. <span data-ttu-id="a732f-235">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a732f-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a732f-237">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a732f-237">Click **Add** button.</span></span> <span data-ttu-id="a732f-238">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a732f-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a732f-240">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a732f-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a732f-241">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a732f-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a732f-242">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a732f-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a732f-243">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a732f-243">Testing single sign-on</span></span>

<span data-ttu-id="a732f-244">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a732f-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a732f-245">Když kliknete na dlaždici Lifesize cloudu na přístupovém panelu, měli byste obdržet přihlašovací stránku Lifesize cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="a732f-245">When you click the Lifesize Cloud tile in the Access Panel, you should get login page of Lifesize Cloud application.</span></span>
<span data-ttu-id="a732f-246">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a732f-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a732f-247">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a732f-247">Additional resources</span></span>

* [<span data-ttu-id="a732f-248">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a732f-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a732f-249">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a732f-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

