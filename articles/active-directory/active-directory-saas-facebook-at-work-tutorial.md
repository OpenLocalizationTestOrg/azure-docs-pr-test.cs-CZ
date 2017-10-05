---
title: "Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook. | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a síti na pracovišti ve službě Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="347fb-103">Kurz: Azure Active Directory integrace s síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="347fb-104">V tomto kurzu zjistěte, jak k síti na pracovišti ve službě Facebook integraci s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="347fb-104">In this tutorial, you learn how to integrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="347fb-105">Integrace síti na pracovišti ve službě Facebook s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="347fb-105">Integrating Workplace by Facebook with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="347fb-106">Můžete ovládat ve službě Azure AD, který má přístup k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-106">You can control in Azure AD who has access to Workplace by Facebook.</span></span>
- <span data-ttu-id="347fb-107">Můžete povolit uživatelům automaticky získat přihlášeného k síti na pracovišti ve službě Facebook (jednotné přihlášení) s jejich účty Azure AD.</span><span class="sxs-lookup"><span data-stu-id="347fb-107">You can enable your users to automatically get signed on to Workplace by Facebook (single sign-on) with their Azure AD accounts.</span></span>
- <span data-ttu-id="347fb-108">Můžete spravovat vaše účty v jednom centrálním místě: portál Azure.</span><span class="sxs-lookup"><span data-stu-id="347fb-108">You can manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="347fb-109">Další podrobnosti o softwaru, služba (SaaS) aplikace integraci s Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="347fb-109">For more details about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="347fb-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="347fb-110">Prerequisites</span></span>

<span data-ttu-id="347fb-111">Ke konfiguraci integrace služby Azure AD se na pracovišti ve službě Facebook, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="347fb-111">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="347fb-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="347fb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="347fb-113">Předplatné povolené firemní síti pomocí sítě Facebook jednotné přihlašování (SSO)</span><span class="sxs-lookup"><span data-stu-id="347fb-113">A Workplace by Facebook single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="347fb-114">Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:</span><span class="sxs-lookup"><span data-stu-id="347fb-114">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="347fb-115">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="347fb-115">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="347fb-116">Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="347fb-116">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="347fb-117">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="347fb-117">Scenario description</span></span>
<span data-ttu-id="347fb-118">V tomto kurzu Azure AD jednotného přihlašování k testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="347fb-118">In this tutorial, you test Azure AD SSO in a test environment.</span></span> <span data-ttu-id="347fb-119">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="347fb-119">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="347fb-120">Přidejte síti na pracovišti ve službě Facebook z galerie.</span><span class="sxs-lookup"><span data-stu-id="347fb-120">Add Workplace by Facebook from the gallery.</span></span>
2. <span data-ttu-id="347fb-121">Konfigurace a otestování Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="347fb-121">Configure and test Azure AD single sign-on.</span></span>

## <a name="add-workplace-by-facebook-from-the-gallery"></a><span data-ttu-id="347fb-122">Přidat pracovní ploše ve službě Facebook z Galerie</span><span class="sxs-lookup"><span data-stu-id="347fb-122">Add Workplace by Facebook from the gallery</span></span>
<span data-ttu-id="347fb-123">Při konfiguraci integrace síti na pracovišti ve službě Facebook do služby Azure AD přidáte do seznamu spravovaných aplikací SaaS síti na pracovišti ve službě Facebook z galerie.</span><span class="sxs-lookup"><span data-stu-id="347fb-123">To configure the integration of Workplace by Facebook into Azure AD, add Workplace by Facebook from the gallery to your list of managed SaaS apps.</span></span>

1. <span data-ttu-id="347fb-124">V [portál Azure](https://portal.azure.com), v levém podokně vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="347fb-124">In the [Azure portal](https://portal.azure.com), in the left pane, select **Azure Active Directory**.</span></span> 

    ![Tlačítko Azure Active Directory][1]

2. <span data-ttu-id="347fb-126">Přejděte do **podnikové aplikace, které** > **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="347fb-126">Browse to **Enterprise applications** > **All applications**.</span></span>

    ![V okně podnikové aplikace][2]
    
3. <span data-ttu-id="347fb-128">Chcete-li přidat novou aplikaci, vyberte **novou aplikaci** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="347fb-128">To add the new application, select **New application** on the top of the dialog box.</span></span>

    ![Tlačítko nové aplikace][3]

4. <span data-ttu-id="347fb-130">Do vyhledávacího pole zadejte **síti na pracovišti ve službě Facebook**a vyberte **síti na pracovišti ve službě Facebook** z výsledků.</span><span class="sxs-lookup"><span data-stu-id="347fb-130">In the search box, type **Workplace by Facebook**, and select **Workplace by Facebook** from results.</span></span> <span data-ttu-id="347fb-131">Potom vyberte **přidat**, chcete-li přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="347fb-131">Then select **Add**, to add the application.</span></span>

    ![Síti na pracovišti ve službě Facebook v seznamu výsledků](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="347fb-133">Konfigurace a otestování Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="347fb-133">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="347fb-134">V této části můžete nakonfigurovat a otestovat Azure AD přihlášení SSO se síti na pracovišti ve službě Facebook, podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="347fb-134">In this section, you configure and test Azure AD SSO with Workplace by Facebook, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="347fb-135">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v síti na pracovišti ve službě Facebook je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="347fb-135">For SSO to work, Azure AD needs to know what the counterpart user in Workplace by Facebook is to a user in Azure AD.</span></span> <span data-ttu-id="347fb-136">Jinými slovy je třeba vytvořit propojené vztah mezi uživatele Azure AD a související uživateli v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-136">In other words, a linked relationship between an Azure AD user and the related user in Workplace by Facebook should be established.</span></span>

<span data-ttu-id="347fb-137">Vytvořit tento vztah přiřazením hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-137">Establish this relationship by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workplace by Facebook.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="347fb-138">Konfigurovat Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="347fb-138">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="347fb-139">V této části povolíte Azure AD jednotného přihlašování k portálu Azure a nakonfigurovat jednotné přihlašování na vašem pracovišti aplikace Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-139">In this section, you enable Azure AD SSO in the Azure portal, and you configure SSO in your Workplace by Facebook application.</span></span>

1. <span data-ttu-id="347fb-140">Na portálu Azure na **síti na pracovišti ve službě Facebook** stránky integrace aplikací, vyberte **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="347fb-140">In the Azure portal, on the **Workplace by Facebook** application integration page, select **Single sign-on**.</span></span>

    ![Konfigurace propojení přihlášení][4]

2. <span data-ttu-id="347fb-142">V **jednotného přihlašování** dialogové okno, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="347fb-142">In the **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on** to enable SSO.</span></span>
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="347fb-144">V **síti na pracovišti Facebook domény a adresy URL** část, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="347fb-144">In the **Workplace by Facebook Domain and URLs** section, do the following:</span></span>

    <span data-ttu-id="347fb-145">a.</span><span class="sxs-lookup"><span data-stu-id="347fb-145">a.</span></span> <span data-ttu-id="347fb-146">V **přihlašovací adresa URL** textového pole zadejte adresu URL, která používá následující vzorec:`https://<company subdomain>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="347fb-146">In the **Sign-on URL** text box, type a URL that uses the following pattern: `https://<company subdomain>.facebook.com`</span></span>

    <span data-ttu-id="347fb-147">b.</span><span class="sxs-lookup"><span data-stu-id="347fb-147">b.</span></span> <span data-ttu-id="347fb-148">V **identifikátor** textového pole zadejte adresu URL, která používá následující vzorec:`https://www.facebook.com/company/<scim company id>`</span><span class="sxs-lookup"><span data-stu-id="347fb-148">In the **Identifier** text box, type a URL that uses the following pattern: `https://www.facebook.com/company/<scim company id>`</span></span>

    > [!NOTE]
    > <span data-ttu-id="347fb-149">Tyto hodnoty jsou pouze příklad.</span><span class="sxs-lookup"><span data-stu-id="347fb-149">These values are an example only.</span></span> <span data-ttu-id="347fb-150">Tyto hodnoty aktualizujte skutečné přihlašovací adresa URL a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="347fb-150">Update these values with the actual sign-on URL and identifier.</span></span> <span data-ttu-id="347fb-151">Obraťte se [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="347fb-151">Contact the [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) to get these values.</span></span> 

4. <span data-ttu-id="347fb-152">V **SAML podpisový certifikát** vyberte **certifikátu (Base64)**a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="347fb-152">In the **SAML Signing Certificate** section, select **Certificate (Base64)**, and then save the certificate file on your computer.</span></span>

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="347fb-154">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="347fb-154">Select **Save**.</span></span>

    ![Nakonfigurujte jeden přihlašování uložit tlačítko](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="347fb-156">V **síti na pracovišti v konfiguraci sítě Facebook** vyberte **pracoviště nakonfigurovat ve službě Facebook** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="347fb-156">In the **Workplace by Facebook Configuration** section, select **Configure Workplace by Facebook** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="347fb-157">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="347fb-157">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference** section.</span></span>

    ![Síti na pracovišti v konfiguraci sítě Facebook](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. <span data-ttu-id="347fb-159">V okně prohlížeče jiný web Přihlaste se k pracovní ploše lokalitou Facebook společnosti jako správce.</span><span class="sxs-lookup"><span data-stu-id="347fb-159">In a different web browser window, sign in to your Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="347fb-160">Jako součást procesu ověřování SAML můžete síti na pracovišti velikost použít řetězce dotazu až 2,5 kilobajtů, aby bylo možné předat parametry do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="347fb-160">As part of the SAML authentication process, Workplace can use query strings of up to 2.5 kilobytes in size in order to pass parameters to Azure AD.</span></span>

8. <span data-ttu-id="347fb-161">V **řídicí panel společnosti**, přejděte na **ověřování** kartě.</span><span class="sxs-lookup"><span data-stu-id="347fb-161">In the **Company Dashboard**, go to the **Authentication** tab.</span></span>

9. <span data-ttu-id="347fb-162">V části **ověřování SAML**, vyberte **pouze jednotné přihlašování** z rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="347fb-162">Under **SAML Authentication**, select **SSO Only** from the drop-down list.</span></span>

10. <span data-ttu-id="347fb-163">Zadejte hodnoty zkopírovaných z **síti na pracovišti v konfiguraci sítě Facebook** části portálu Azure do odpovídajícího pole:</span><span class="sxs-lookup"><span data-stu-id="347fb-163">Enter the values copied from the **Workplace by Facebook Configuration** section of the Azure portal into the corresponding fields:</span></span>

    *   <span data-ttu-id="347fb-164">V **SAML URL** text vložte hodnotu **jeden přihlašování adresa URL služby**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="347fb-164">In the **SAML URL** text box, paste the value of **Single Sign-On Service URL**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="347fb-165">V **URL vystavitele SAML** textové pole, vložte hodnotu **SAML Entity ID**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="347fb-165">In the **SAML Issuer URL** text box, paste the value of **SAML Entity ID**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="347fb-166">V **SAML odhlášení přesměrovat (volitelné)**, vložte hodnotu **Sign-Out URL**, který jste zkopírovali z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="347fb-166">In **SAML Logout Redirect (optional)**, paste the value of **Sign-Out URL**, which you have copied from the Azure portal.</span></span>
    *   <span data-ttu-id="347fb-167">Otevřete váš **certifikátu s kódováním base-64** v poznámkovém bloku stáhli z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="347fb-167">Open your **base-64 encoded certificate** in Notepad, downloaded from the Azure portal.</span></span> <span data-ttu-id="347fb-168">Zkopírujte obsah ho do schránky a vložte jej do **certifikátu SAML** textové pole.</span><span class="sxs-lookup"><span data-stu-id="347fb-168">Copy the content of it into your clipboard, and then paste it to the **SAML Certificate** text box.</span></span>

11. <span data-ttu-id="347fb-169">Možná budete muset zadat adresu URL cílové skupiny příjemce adresy URL a adresy URL služby ACS (služba Assertion příjemce), uvedené v části **konfigurace SAML** části.</span><span class="sxs-lookup"><span data-stu-id="347fb-169">You might need to enter the audience URL, recipient URL, and ACS (Assertion Consumer Service) URL, listed under the **SAML Configuration** section.</span></span>

12. <span data-ttu-id="347fb-170">Přejděte do dolní části a vyberte **Test jednotného přihlašování k**.</span><span class="sxs-lookup"><span data-stu-id="347fb-170">Scroll to the bottom of the section, and select **Test SSO**.</span></span> <span data-ttu-id="347fb-171">Automaticky otevírané okno se zobrazí se na přihlašovací stránku služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="347fb-171">A pop-up window appears, with the Azure AD sign-in page.</span></span> <span data-ttu-id="347fb-172">K ověření, zadejte přihlašovací údaje jako normální.</span><span class="sxs-lookup"><span data-stu-id="347fb-172">To authenticate, enter your credentials as normal.</span></span> <span data-ttu-id="347fb-173">Ujistěte se, e-mailovou adresu vrátil zpět z Azure AD je stejný jako pracovní účet, který jste se přihlásili.</span><span class="sxs-lookup"><span data-stu-id="347fb-173">Ensure the email address returned back from Azure AD is the same as the Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="347fb-174">Pokud se test byl úspěšně dokončen, přejděte do dolní části stránky a vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="347fb-174">If the test has finished successfully, scroll to the bottom of the page and select **Save**.</span></span>

14. <span data-ttu-id="347fb-175">Každý, kdo používá síti na pracovišti se teď zobrazí s Azure AD přihlašovací stránka pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="347fb-175">Anyone using Workplace is now presented with Azure AD sign-in page for authentication.</span></span>

<span data-ttu-id="347fb-176">Můžete nakonfigurovat přihlašování SAML adresu URL, který můžete použít tak, aby odkazoval na odhlašovací stránky Azure AD.</span><span class="sxs-lookup"><span data-stu-id="347fb-176">You can choose to configure a SAML sign out URL, which can be used to point at the Azure AD sign-out page.</span></span> <span data-ttu-id="347fb-177">Pokud je toto nastavení povolené a nakonfigurované, uživatel se přesměruje už k síti na pracovišti odhlašovací stránky.</span><span class="sxs-lookup"><span data-stu-id="347fb-177">When this setting is enabled and configured, the user is no longer directed to the Workplace sign-out page.</span></span> <span data-ttu-id="347fb-178">Místo toho se uživatel přesměruje na adresu URL, která byla přidána do nastavení odhlášení přesměrování SAML.</span><span class="sxs-lookup"><span data-stu-id="347fb-178">Instead, the user is redirected to the URL that was added in the SAML sign-out redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="347fb-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco k nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="347fb-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app.</span></span> <span data-ttu-id="347fb-180">Po přidání této aplikace z **služby Active Directory** > **podnikové aplikace, které** jednoduše vyberte **jednotné přihlašování** kartě a přístup vložené dokumentace prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="347fb-180">After adding this app from the **Active Directory** > **Enterprise Applications** section, simply select the **Single Sign-On** tab, and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="347fb-181">Si můžete přečíst informace o funkci embedded dokumentace v [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="347fb-181">You can read more about the embedded documentation feature in the [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="configure-reauthentication-frequency"></a><span data-ttu-id="347fb-182">Nakonfigurovat četnost opětovné ověření</span><span class="sxs-lookup"><span data-stu-id="347fb-182">Configure reauthentication frequency</span></span>

<span data-ttu-id="347fb-183">Můžete nakonfigurovat síti na pracovišti, aby se zobrazila výzva pro kontrolu SAML každý den, tři dny jeden týden, dva týdny, jeden měsíc, nebo vůbec.</span><span class="sxs-lookup"><span data-stu-id="347fb-183">You can configure Workplace to prompt for a SAML check every day, three days, one week, two weeks, one month, or never.</span></span>

> [!NOTE] 
><span data-ttu-id="347fb-184">Minimální hodnota pro kontrolu SAML na mobilní aplikace nastavena na jeden týden.</span><span class="sxs-lookup"><span data-stu-id="347fb-184">The minimum value for the SAML check on mobile applications is set to one week.</span></span>

<span data-ttu-id="347fb-185">Můžete taky přinutit SAML obnovit pro všechny uživatele.</span><span class="sxs-lookup"><span data-stu-id="347fb-185">You can also force a SAML reset for all users.</span></span> <span data-ttu-id="347fb-186">Chcete-li to provést, použijte **vyžadovat ověřování SAML pro všechny uživatele nyní** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="347fb-186">To do this, use the **Require SAML authentication for all users now** button.</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="347fb-187">Vytvořit testovací uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="347fb-187">Create an Azure AD test user</span></span>
<span data-ttu-id="347fb-188">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="347fb-188">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

1. <span data-ttu-id="347fb-190">V **portál Azure**, v levém podokně vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="347fb-190">In the **Azure portal**, in the left pane, select **Azure Active Directory**.</span></span>

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="347fb-192">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a vyberte **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="347fb-192">To display the list of users, go to **Users and groups**, and select **All users**.</span></span>
    
    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="347fb-194">Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="347fb-194">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Tlačítko Přidat](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="347fb-196">V **uživatele** dialogové okno pole, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="347fb-196">In the **User** dialog box, do the following:</span></span>
 
    ![Dialogové okno uživatele](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="347fb-198">a.</span><span class="sxs-lookup"><span data-stu-id="347fb-198">a.</span></span> <span data-ttu-id="347fb-199">V **název** textového pole, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="347fb-199">In the **Name** text box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="347fb-200">b.</span><span class="sxs-lookup"><span data-stu-id="347fb-200">b.</span></span> <span data-ttu-id="347fb-201">V **uživatelské jméno** textového pole, zadejte **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="347fb-201">In the **User name** text box, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="347fb-202">c.</span><span class="sxs-lookup"><span data-stu-id="347fb-202">c.</span></span> <span data-ttu-id="347fb-203">Vyberte **zobrazit hesla**a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="347fb-203">Select **Show Password**, and write it down.</span></span>

    <span data-ttu-id="347fb-204">d.</span><span class="sxs-lookup"><span data-stu-id="347fb-204">d.</span></span> <span data-ttu-id="347fb-205">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="347fb-205">Select **Create**.</span></span>
 
### <a name="create-a-workplace-by-facebook-test-user"></a><span data-ttu-id="347fb-206">Vytvoření pracovišti Facebook testovací uživatel</span><span class="sxs-lookup"><span data-stu-id="347fb-206">Create a Workplace by Facebook test user</span></span>

<span data-ttu-id="347fb-207">V této části uživatele volat Britta Simon vytvoří v síti na pracovišti Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-207">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="347fb-208">Síti na pracovišti ve službě Facebook podporuje za běhu zřizování, který je ve výchozím nastavení povolené.</span><span class="sxs-lookup"><span data-stu-id="347fb-208">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="347fb-209">Neexistuje žádná akce pro vás v této části.</span><span class="sxs-lookup"><span data-stu-id="347fb-209">There is no action for you in this section.</span></span> <span data-ttu-id="347fb-210">Pokud uživatel neexistuje v síti na pracovišti ve službě Facebook, nový vytvoří se při pokusu o přístup k síti na pracovišti Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-210">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt to access Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="347fb-211">Pokud je potřeba ručně vytvořit uživateli, obraťte se [síti na pracovišti tým podpory klienta sítě Facebook](https://workplace.fb.com/faq/).</span><span class="sxs-lookup"><span data-stu-id="347fb-211">If you need to create a user manually, contact the [Workplace by Facebook Client support team](https://workplace.fb.com/faq/).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="347fb-212">Přiřadit testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="347fb-212">Assign the Azure AD test user</span></span>

<span data-ttu-id="347fb-213">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k síti na pracovišti ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="347fb-213">In this section, you enable Britta Simon to use Azure SSO by granting access to Workplace by Facebook.</span></span>

![Přiřadit uživatele][200] 

1. <span data-ttu-id="347fb-215">Na portálu Azure otevřete zobrazení aplikace.</span><span class="sxs-lookup"><span data-stu-id="347fb-215">In the Azure portal, open the applications view.</span></span> <span data-ttu-id="347fb-216">Přejděte do zobrazení adresáře, přejděte na **podnikové aplikace, které**a potom vyberte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="347fb-216">Go to the directory view, go to **Enterprise applications**, and then select **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="347fb-218">V seznamu aplikací vyberte **síti na pracovišti ve službě Facebook**.</span><span class="sxs-lookup"><span data-stu-id="347fb-218">In the applications list, select **Workplace by Facebook**.</span></span>

    ![K síti na pracovišti podle propojení sítě Facebook v seznamu aplikací](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="347fb-220">V nabídce na levé straně vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="347fb-220">In the menu on the left, select **Users and groups**.</span></span>

    ![Odkaz "Uživatelé a skupiny"][202] 

4. <span data-ttu-id="347fb-222">Vyberte **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="347fb-222">Select **Add**.</span></span> <span data-ttu-id="347fb-223">Potom v **přidat přiřazení** podokně, vyberte **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="347fb-223">Then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![V podokně Přidat přiřazení][203]

5. <span data-ttu-id="347fb-225">V **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="347fb-225">In the **Users and groups** dialog box, select **Britta Simon** in the users list.</span></span>

6. <span data-ttu-id="347fb-226">V **uživatelů a skupin** dialogové okno, vyberte **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="347fb-226">In the **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="347fb-227">V **přidat přiřazení** dialogové okno, vyberte **přiřadit**.</span><span class="sxs-lookup"><span data-stu-id="347fb-227">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="347fb-228">Test jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="347fb-228">Test single sign-on</span></span>

<span data-ttu-id="347fb-229">Pokud chcete otestovat nastavení jednotného přihlašování, otevřete Panel přístupu.</span><span class="sxs-lookup"><span data-stu-id="347fb-229">If you want to test your SSO settings, open the Access Panel.</span></span>
<span data-ttu-id="347fb-230">Další informace najdete v tématu [Úvod do přístupového panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="347fb-230">For more information, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="347fb-231">Další kroky</span><span class="sxs-lookup"><span data-stu-id="347fb-231">Next steps</span></span>

* <span data-ttu-id="347fb-232">Najdete v článku [seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md).</span><span class="sxs-lookup"><span data-stu-id="347fb-232">See the [list of tutorials on how to integrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md).</span></span>
* <span data-ttu-id="347fb-233">Čtení [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="347fb-233">Read [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>
* <span data-ttu-id="347fb-234">Další informace o tom, jak [konfiguraci zřizování uživatelů](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="347fb-234">Learn more about how to [configure user provisioning](active-directory-saas-facebook-at-work-provisioning-tutorial.md).</span></span>



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

