---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1590a66f215f0c093d24ff602c0ad951ba1e1eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="87020-103">Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="87020-104">V tomto kurzu zjistěte, jak k síti na pracovišti ve službě Facebook integraci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="87020-104">In this tutorial, you learn how to integrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="87020-105">Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="87020-105">Integrating Workplace by Facebook with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="87020-106">Můžete řídit ve službě Azure AD, který má přístup k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-106">You can control in Azure AD who has access to Workplace by Facebook</span></span>
- <span data-ttu-id="87020-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k síti na pracovišti ve službě Facebook (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="87020-107">You can enable your users to automatically get signed-on to Workplace by Facebook (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="87020-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="87020-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="87020-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="87020-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="87020-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="87020-110">Prerequisites</span></span>

<span data-ttu-id="87020-111">Ke konfiguraci integrace služby Azure AD se na pracovišti ve službě Facebook, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="87020-111">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="87020-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="87020-112">An Azure AD subscription</span></span>
- <span data-ttu-id="87020-113">Firemní síti pomocí sítě Facebook jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="87020-113">A Workplace by Facebook single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="87020-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="87020-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="87020-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="87020-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="87020-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="87020-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="87020-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87020-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="87020-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="87020-118">Scenario description</span></span>
<span data-ttu-id="87020-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="87020-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="87020-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="87020-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="87020-121">Přidání síti na pracovišti ve službě Facebook z Galerie</span><span class="sxs-lookup"><span data-stu-id="87020-121">Adding Workplace by Facebook from the gallery</span></span>
2. <span data-ttu-id="87020-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="87020-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workplace-by-facebook-from-the-gallery"></a><span data-ttu-id="87020-123">Přidání síti na pracovišti ve službě Facebook z Galerie</span><span class="sxs-lookup"><span data-stu-id="87020-123">Adding Workplace by Facebook from the gallery</span></span>
<span data-ttu-id="87020-124">Při konfiguraci integrace síti na pracovišti ve službě Facebook do služby Azure AD potřebujete přidat síti na pracovišti ve službě Facebook z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="87020-124">To configure the integration of Workplace by Facebook into Azure AD, you need to add Workplace by Facebook from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="87020-125">**Pokud chcete přidat síti na pracovišti ve službě Facebook z galerie, postupujte takto:**</span><span class="sxs-lookup"><span data-stu-id="87020-125">**To add Workplace by Facebook from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="87020-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="87020-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="87020-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="87020-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="87020-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87020-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="87020-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87020-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="87020-133">Do vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="87020-133">In the search box, type **Workplace by Facebook**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. <span data-ttu-id="87020-135">Na panelu výsledků vyberte **síti na pracovišti ve službě Facebook**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="87020-135">In the results panel, select **Workplace by Facebook**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="87020-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="87020-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="87020-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování se na pracovišti ve službě Facebook podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="87020-138">In this section, you configure and test Azure AD single sign-on with Workplace by Facebook based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="87020-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v síti na pracovišti ve službě Facebook je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87020-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workplace by Facebook is to a user in Azure AD.</span></span> <span data-ttu-id="87020-140">Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživateli v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-140">In other words, a link relationship between an Azure AD user and the related user in Workplace by Facebook needs to be established.</span></span>

<span data-ttu-id="87020-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workplace by Facebook.</span></span>

<span data-ttu-id="87020-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování se na pracovišti ve službě Facebook, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="87020-142">To configure and test Azure AD single sign-on with Workplace by Facebook, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="87020-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="87020-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="87020-144">**[Konfiguraci frekvence opětovné ověření](#configuring-reauthentication-frequency)**  – Pokud chcete nakonfigurovat síti na pracovišti, aby se zobrazila výzva pro kontrolu SAML.</span><span class="sxs-lookup"><span data-stu-id="87020-144">**[Configuring Reauthentication Frequency](#configuring-reauthentication-frequency)** - to configure Workplace to prompt for a SAML check.</span></span>
3. <span data-ttu-id="87020-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87020-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="87020-146">**[Vytváření pracovišti uživatelem testovací Facebook](#creating-a-workplace-by-facebook-test-user)**  – Pokud chcete mít protějšek Britta Simon v síti na pracovišti podle Facebook, propojené služby Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="87020-146">**[Creating a Workplace by Facebook test user](#creating-a-workplace-by-facebook-test-user)** - to have a counterpart of Britta Simon in Workplace by Facebook that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="87020-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="87020-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="87020-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="87020-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="87020-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="87020-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="87020-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workplace by Facebook application.</span></span>

<span data-ttu-id="87020-151">**Ke konfiguraci Azure AD jednotné přihlašování se na pracovišti ve službě Facebook, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87020-151">**To configure Azure AD single sign-on with Workplace by Facebook, perform the following steps:**</span></span>

1. <span data-ttu-id="87020-152">Na portálu Azure na **síti na pracovišti ve službě Facebook** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="87020-152">In the Azure portal, on the **Workplace by Facebook** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="87020-154">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="87020-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="87020-156">Na **síti na pracovišti Facebook domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87020-156">On the **Workplace by Facebook Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    <span data-ttu-id="87020-158">a.</span><span class="sxs-lookup"><span data-stu-id="87020-158">a.</span></span> <span data-ttu-id="87020-159">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<instancename>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="87020-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.facebook.com`</span></span>

    <span data-ttu-id="87020-160">b.</span><span class="sxs-lookup"><span data-stu-id="87020-160">b.</span></span> <span data-ttu-id="87020-161">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://www.facebook.com/company/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="87020-161">In the **Identifier** textbox, type a URL using the following pattern: `https://www.facebook.com/company/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="87020-162">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="87020-162">These values are not the real.</span></span> <span data-ttu-id="87020-163">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="87020-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="87020-164">Obraťte se na [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="87020-164">Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) to get these values.</span></span> 

4. <span data-ttu-id="87020-165">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="87020-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="87020-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87020-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="87020-169">Na **síti na pracovišti v konfiguraci sítě Facebook** klikněte na tlačítko **pracoviště nakonfigurovat ve službě Facebook** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="87020-169">On the **Workplace by Facebook Configuration** section, click **Configure Workplace by Facebook** to open **Configure sign-on** window.</span></span> <span data-ttu-id="87020-170">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="87020-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. <span data-ttu-id="87020-172">V okně prohlížeče jiný web a přihlášení k pracovní ploše lokalitou Facebook společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="87020-172">In a different web browser window, login to your Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="87020-173">Jako součást procesu ověřování SAML může síti na pracovišti využívat řetězce dotazu až 2,5 kilobajtů velikost, aby bylo možné předat parametry do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87020-173">As part of the SAML authentication process, Workplace may utilize query strings of up to 2.5 kilobytes in size in order to pass parameters to Azure AD.</span></span>

8. <span data-ttu-id="87020-174">V **řídicí panel společnosti**, přejděte na **ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="87020-174">In the **Company Dashboard**, go to the **Authentication** tab.</span></span>

9. <span data-ttu-id="87020-175">V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="87020-175">Under **SAML Authentication**, select **SSO Only** from the drop-down list.</span></span>

10. <span data-ttu-id="87020-176">Vstupní hodnoty zkopírovaných z **síti na pracovišti v konfiguraci sítě Facebook** části portálu Azure do odpovídajícího pole:</span><span class="sxs-lookup"><span data-stu-id="87020-176">Input the values copied from **Workplace by Facebook Configuration** section of the Azure portal into the corresponding fields:</span></span>

    *   <span data-ttu-id="87020-177">V **SAML URL** textovému poli, vložte hodnotu **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87020-177">In **SAML URL** textbox, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="87020-178">V **URL vystavitele SAML textbox**, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87020-178">In **SAML Issuer URL textbox**, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="87020-179">V **přesměrování odhlašovací SAML** (volitelné), vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="87020-179">In **SAML Logout Redirect** (Optional), paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="87020-180">Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure, kopírovat obsah ho do schránky a vložte jej do **certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="87020-180">Open your **base-64 encoded certificate** in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **SAML Certificate** textbox.</span></span>

11. <span data-ttu-id="87020-181">Budete muset zadat adresu URL cílové skupiny, adresa URL příjemce, a adresa URL služby ACS (služba Assertion příjemce) uvedené v části **konfigurace SAML** části.</span><span class="sxs-lookup"><span data-stu-id="87020-181">You may need to enter the Audience URL, Recipient URL, and ACS (Assertion Consumer Service) URL listed under the **SAML Configuration** section.</span></span>

12. <span data-ttu-id="87020-182">Přejděte do dolní části a klikněte na **Test jednotného přihlašování k** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87020-182">Scroll to the bottom of the section and click the **Test SSO** button.</span></span> <span data-ttu-id="87020-183">Zobrazí se tato výsledky v automaticky otevíraném okně, zobrazí se přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="87020-183">This results in a popup window appearing with Azure AD login page presented.</span></span> <span data-ttu-id="87020-184">Zadejte přihlašovací údaje v jako normální k ověření.</span><span class="sxs-lookup"><span data-stu-id="87020-184">Enter your credentials in as normal to authenticate.</span></span> 

    <span data-ttu-id="87020-185">**Řešení potíží:** zkontrolujte e-mailová adresa se vrací zpět z Azure AD je stejný jako pracovní účet, který jste se přihlásili.</span><span class="sxs-lookup"><span data-stu-id="87020-185">**Troubleshooting:** Ensure the email address being returned back from Azure AD is the same as the Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="87020-186">Po úspěšném dokončení testu, přejděte do dolní části stránky a klikněte na **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87020-186">Once the test has been completed successfully, scroll to the bottom of the page and click the **Save** button.</span></span>

14. <span data-ttu-id="87020-187">Všichni uživatelé používali síti na pracovišti nyní zobrazí se přihlašovací stránku služby Azure AD pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="87020-187">All users using Workplace will now be presented with Azure AD login page for authentication.</span></span>

15. <span data-ttu-id="87020-188">**SAML odhlášení přesměrovat (volitelné)** -</span><span class="sxs-lookup"><span data-stu-id="87020-188">**SAML Logout Redirect (optional)** -</span></span> 

    <span data-ttu-id="87020-189">Můžete volitelně lze konfigurovat adresu Url odhlašovací SAML, který můžete použít tak, aby odkazoval na Azure AD odhlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="87020-189">You can choose to optionally configure a SAML Logout Url, which can be used to point at Azure AD's logout page.</span></span> <span data-ttu-id="87020-190">Pokud je toto nastavení povolené a nakonfigurované, se uživatel přesměruje už k síti na pracovišti odhlašovací stránce.</span><span class="sxs-lookup"><span data-stu-id="87020-190">When this setting is enabled and configured, the user will no longer be directed to the Workplace logout page.</span></span> <span data-ttu-id="87020-191">Místo toho bude uživatel přesměrován na adresu url, která byla přidána do nastavení přesměrování odhlašovací SAML.</span><span class="sxs-lookup"><span data-stu-id="87020-191">Instead, the user will be redirected to the url that was added in the SAML Logout Redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="87020-192">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="87020-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="87020-193">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="87020-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="87020-194">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="87020-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="configuring-reauthentication-frequency"></a><span data-ttu-id="87020-195">Konfiguraci frekvence opětovné ověření</span><span class="sxs-lookup"><span data-stu-id="87020-195">Configuring Reauthentication Frequency</span></span>

<span data-ttu-id="87020-196">Můžete nakonfigurovat síti na pracovišti vyžadovat kontrolu SAML každý den, tři dny týdne dvou týdnů, měsíců nebo vůbec.</span><span class="sxs-lookup"><span data-stu-id="87020-196">You can configure Workplace to prompt for a SAML check every day, three days, week, two weeks, month or never.</span></span>

> [!NOTE] 
><span data-ttu-id="87020-197">Minimální hodnota pro kontrolu SAML na mobilní aplikace nastavena na jeden týden.</span><span class="sxs-lookup"><span data-stu-id="87020-197">The minimum value for the SAML check on mobile applications is set to one week.</span></span>

<span data-ttu-id="87020-198">Můžete taky přinutit SAML obnovit pro všechny uživatele pomocí tlačítka: vyžadovat ověřování SAML pro všechny uživatele nyní.</span><span class="sxs-lookup"><span data-stu-id="87020-198">You can also force a SAML reset for all users using the button: Require SAML authentication for all users now.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="87020-199">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="87020-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="87020-200">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="87020-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="87020-202">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87020-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="87020-203">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="87020-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="87020-205">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="87020-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="87020-207">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87020-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="87020-209">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="87020-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="87020-211">a.</span><span class="sxs-lookup"><span data-stu-id="87020-211">a.</span></span> <span data-ttu-id="87020-212">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="87020-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="87020-213">b.</span><span class="sxs-lookup"><span data-stu-id="87020-213">b.</span></span> <span data-ttu-id="87020-214">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="87020-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="87020-215">c.</span><span class="sxs-lookup"><span data-stu-id="87020-215">c.</span></span> <span data-ttu-id="87020-216">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="87020-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="87020-217">d.</span><span class="sxs-lookup"><span data-stu-id="87020-217">d.</span></span> <span data-ttu-id="87020-218">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="87020-218">Click **Create**.</span></span>
 
### <a name="creating-a-workplace-by-facebook-test-user"></a><span data-ttu-id="87020-219">Vytváření pracovišti Facebook testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="87020-219">Creating a Workplace by Facebook test user</span></span>

<span data-ttu-id="87020-220">V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-220">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="87020-221">Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="87020-221">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="87020-222">Neexistuje žádná akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="87020-222">There is no action for you in this section.</span></span> <span data-ttu-id="87020-223">Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, nový vytvoří se při pokusu o přístup k síti na pracovišti Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-223">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt to access Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="87020-224">Pokud je potřeba ručně vytvořit uživatele, obraťte se na [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/)</span><span class="sxs-lookup"><span data-stu-id="87020-224">If you need to create a user manually, Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/)</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="87020-225">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="87020-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="87020-226">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="87020-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workplace by Facebook.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="87020-228">**Britta Simon přiřadit k firemní síti pomocí sítě Facebook, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="87020-228">**To assign Britta Simon to Workplace by Facebook, perform the following steps:**</span></span>

1. <span data-ttu-id="87020-229">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="87020-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="87020-231">V seznamu aplikací vyberte **síti na pracovišti ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="87020-231">In the applications list, select **Workplace by Facebook**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="87020-233">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="87020-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="87020-235">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="87020-235">Click **Add** button.</span></span> <span data-ttu-id="87020-236">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87020-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="87020-238">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="87020-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="87020-239">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87020-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="87020-240">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="87020-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="87020-241">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="87020-241">Testing single sign-on</span></span>

<span data-ttu-id="87020-242">Pokud chcete testovat vaše nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="87020-242">If you want to test your single sign-on settings, open the Access Panel.</span></span>
<span data-ttu-id="87020-243">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="87020-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="87020-244">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="87020-244">Additional resources</span></span>

* [<span data-ttu-id="87020-245">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="87020-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="87020-246">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="87020-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="87020-247">Konfiguraci zřizování uživatelů</span><span class="sxs-lookup"><span data-stu-id="87020-247">Configure User Provisioning</span></span>](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

