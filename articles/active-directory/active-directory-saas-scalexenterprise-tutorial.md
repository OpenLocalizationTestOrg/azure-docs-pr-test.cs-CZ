---
title: 'Kurz: Azure Active Directory integrace s ScaleX Enterprise | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ScaleX Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: 0ebed0c2605862426384c0e219e52c9d626b6246
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a><span data-ttu-id="9682c-103">Kurz: Azure Active Directory integrace s ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="9682c-103">Tutorial: Azure Active Directory integration with ScaleX Enterprise</span></span>

<span data-ttu-id="9682c-104">V tomto kurzu zjistěte, jak integrovat ScaleX Enterprise s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9682c-104">In this tutorial, you learn how to integrate ScaleX Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9682c-105">Integrace ScaleX Enterprise s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9682c-105">Integrating ScaleX Enterprise with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9682c-106">Můžete řídit ve službě Azure AD, který má přístup do firemní sítě ScaleX</span><span class="sxs-lookup"><span data-stu-id="9682c-106">You can control in Azure AD who has access to ScaleX Enterprise</span></span>
- <span data-ttu-id="9682c-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ScaleX Enterprise (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9682c-107">You can enable your users to automatically get signed-on to ScaleX Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9682c-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9682c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9682c-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, přečtěte si téma.</span><span class="sxs-lookup"><span data-stu-id="9682c-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="9682c-110">Co je přístup k aplikaci a jednotné přihlašování s [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9682c-110">What is application access and single sign-on with [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9682c-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9682c-111">Prerequisites</span></span>

<span data-ttu-id="9682c-112">Konfigurace integrace Azure AD s ScaleX Enterprise, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9682c-112">To configure Azure AD integration with ScaleX Enterprise, you need the following items:</span></span>

- <span data-ttu-id="9682c-113">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9682c-113">An Azure AD subscription</span></span>
- <span data-ttu-id="9682c-114">ScaleX Enterprise jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9682c-114">A ScaleX Enterprise single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9682c-115">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9682c-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9682c-116">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9682c-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9682c-117">Nepoužívejte provozním prostředí, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="9682c-117">Do not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9682c-118">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9682c-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9682c-119">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9682c-119">Scenario description</span></span>
<span data-ttu-id="9682c-120">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9682c-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9682c-121">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9682c-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9682c-122">Přidání ScaleX Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="9682c-122">Adding ScaleX Enterprise from the gallery</span></span>
2. <span data-ttu-id="9682c-123">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9682c-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scalex-enterprise-from-the-gallery"></a><span data-ttu-id="9682c-124">Přidání ScaleX Enterprise z Galerie</span><span class="sxs-lookup"><span data-stu-id="9682c-124">Adding ScaleX Enterprise from the gallery</span></span>
<span data-ttu-id="9682c-125">Při konfiguraci integrace ScaleX organizace v Azure AD, potřebujete přidat ScaleX Enterprise z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9682c-125">To configure the integration of ScaleX Enterprise in to Azure AD, you need to add ScaleX Enterprise from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9682c-126">**Pokud chcete přidat ScaleX Enterprise z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9682c-126">**To add ScaleX Enterprise from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9682c-127">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9682c-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9682c-129">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9682c-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9682c-130">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9682c-130">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9682c-132">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9682c-132">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9682c-134">Do vyhledávacího pole zadejte **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="9682c-134">In the search box, type **ScaleX Enterprise**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. <span data-ttu-id="9682c-136">Na panelu výsledků vyberte **ScaleX Enterprise**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9682c-136">In the results panel, select **ScaleX Enterprise**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9682c-138">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9682c-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9682c-139">V této části nakonfigurujete a testovací Azure AD jednotného přihlašování k ScaleX organizace podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9682c-139">In this section, you configure and test Azure AD single sign-on with ScaleX Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9682c-140">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ScaleX Enterprise je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9682c-140">For single sign-on to work, Azure AD needs to know what the counterpart user in ScaleX Enterprise is to a user in Azure AD.</span></span> <span data-ttu-id="9682c-141">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatele v podniku ScaleX musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9682c-141">In other words, a link relationship between an Azure AD user and the related user in ScaleX Enterprise needs to be established.</span></span>

<span data-ttu-id="9682c-142">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** ve ScaleX podniku.</span><span class="sxs-lookup"><span data-stu-id="9682c-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ScaleX Enterprise.</span></span>

<span data-ttu-id="9682c-143">Nakonfigurovat a otestovat Azure AD jednotného přihlašování k ScaleX organizace, budete muset provést následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9682c-143">To configure and test Azure AD single sign-on with ScaleX Enterprise, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9682c-144">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9682c-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9682c-145">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9682c-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9682c-146">**[Vytvoření zkušebního uživatele ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  – Pokud chcete mít protějšek Britta Simon v ScaleX organizace, která je propojený s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9682c-146">**[Creating a ScaleX Enterprise test user](#creating-a-scalex-enterprise-test-user)** - to have a counterpart of Britta Simon in ScaleX Enterprise that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9682c-147">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9682c-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9682c-148">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9682c-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9682c-149">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9682c-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9682c-150">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="9682c-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ScaleX Enterprise application.</span></span>

<span data-ttu-id="9682c-151">**Ke konfiguraci Azure AD jednotného přihlašování k ScaleX organizace, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9682c-151">**To configure Azure AD single sign-on with ScaleX Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="9682c-152">Na portálu Azure na **ScaleX Enterprise** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9682c-152">In the Azure portal, on the **ScaleX Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9682c-154">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9682c-154">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. <span data-ttu-id="9682c-156">Na **ScaleX podnikové domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="9682c-156">On the **ScaleX Enterprise Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    <span data-ttu-id="9682c-158">a.</span><span class="sxs-lookup"><span data-stu-id="9682c-158">a.</span></span> <span data-ttu-id="9682c-159">V **identifikátor** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://platform.rescale.com/saml2/<company id>/`</span><span class="sxs-lookup"><span data-stu-id="9682c-159">In the **Identifier** textbox, type the value using the following pattern: `https://platform.rescale.com/saml2/<company id>/`</span></span>

    <span data-ttu-id="9682c-160">b.</span><span class="sxs-lookup"><span data-stu-id="9682c-160">b.</span></span> <span data-ttu-id="9682c-161">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://platform.rescale.com/saml2/<company id>/acs/`</span><span class="sxs-lookup"><span data-stu-id="9682c-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.rescale.com/saml2/<company id>/acs/`</span></span>

4. <span data-ttu-id="9682c-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="9682c-162">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    <span data-ttu-id="9682c-164">V **přihlašovací adresa URL** textovému poli, zadejte hodnotu pomocí následujícího vzorce:`https://platform.rescale.com/saml2/<company id>/sso/`</span><span class="sxs-lookup"><span data-stu-id="9682c-164">In the **Sign-on URL** textbox, type the value using the following pattern: `https://platform.rescale.com/saml2/<company id>/sso/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="9682c-165">Tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9682c-165">These are not the real values.</span></span> <span data-ttu-id="9682c-166">Tyto hodnoty aktualizujte pomocí skutečného identifikátoru, adresa URL odpovědi nebo přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="9682c-166">Update these values with the actual Identifier, Reply URL or Sign-On URL.</span></span> <span data-ttu-id="9682c-167">Obraťte se na [tým podpory klient systému Enterprise ScaleX](http://info.rescale.com/contact_sales) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="9682c-167">Contact [ScaleX Enterprise Client support team](http://info.rescale.com/contact_sales) to get these values.</span></span> 

5. <span data-ttu-id="9682c-168">Aplikace ScaleX očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, abyste upravit mapování vlastních atributů ke konfiguraci atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="9682c-168">Your ScaleX application expects the SAML assertions in a specific format, which requires you to modify custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="9682c-169">Klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** chcete otevřít vlastní atributy nastavení.</span><span class="sxs-lookup"><span data-stu-id="9682c-169">Click **View and edit all other user attributes** checkbox to open the custom attributes settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    <span data-ttu-id="9682c-171">a.</span><span class="sxs-lookup"><span data-stu-id="9682c-171">a.</span></span> <span data-ttu-id="9682c-172">Klikněte pravým tlačítkem na atribut **název** a klikněte na příkaz odstranit.</span><span class="sxs-lookup"><span data-stu-id="9682c-172">Right click the attribute **name** and click delete.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    <span data-ttu-id="9682c-174">b.</span><span class="sxs-lookup"><span data-stu-id="9682c-174">b.</span></span> <span data-ttu-id="9682c-175">Klikněte na tlačítko **emailaddress** atribut a otevřete okno Úprava atributů.</span><span class="sxs-lookup"><span data-stu-id="9682c-175">Click **emailaddress** attribute to open the Edit Attribute window.</span></span> <span data-ttu-id="9682c-176">Změnit svou hodnotu z **user.mail** k **user.userprincipalname** a klikněte na tlačítko Ok.</span><span class="sxs-lookup"><span data-stu-id="9682c-176">Change its value from **user.mail** to **user.userprincipalname** and click Ok.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. <span data-ttu-id="9682c-178">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9682c-178">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. <span data-ttu-id="9682c-180">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9682c-180">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="9682c-182">Na **ScaleX podniková konfigurace** klikněte na tlačítko **konfigurace ScaleX Enterprise** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9682c-182">On the **ScaleX Enterprise Configuration** section, click **Configure ScaleX Enterprise** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9682c-183">Kopírování **SAML Entity ID** a **SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9682c-183">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. <span data-ttu-id="9682c-185">Konfigurace jednotného přihlašování na **ScaleX Enterprise** straně, přihlášení na web společnosti ScaleX Enterprise jako správce.</span><span class="sxs-lookup"><span data-stu-id="9682c-185">To configure single sign-on on **ScaleX Enterprise** side, login to the ScaleX Enterprise company website as an administrator.</span></span>

9. <span data-ttu-id="9682c-186">Klikněte na nabídku v horním pravém rohu a vyberte **Contoso správy**.</span><span class="sxs-lookup"><span data-stu-id="9682c-186">Click the menu in the upper right and select **Contoso Administration**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9682c-187">Contoso je jenom jako příklad.</span><span class="sxs-lookup"><span data-stu-id="9682c-187">Contoso is just an example.</span></span> <span data-ttu-id="9682c-188">To by měl být skutečný název vaší společnosti.</span><span class="sxs-lookup"><span data-stu-id="9682c-188">This should be your actual Company Name.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. <span data-ttu-id="9682c-190">Vyberte **integrace** z horní nabídce a vyberte **jednotné přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9682c-190">Select **Integrations** from the top menu and select **Single Sign-On**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. <span data-ttu-id="9682c-192">Vyplňte formulář následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9682c-192">Complete the form as follows:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    <span data-ttu-id="9682c-194">a.</span><span class="sxs-lookup"><span data-stu-id="9682c-194">a.</span></span> <span data-ttu-id="9682c-195">Vyberte **"Vytvořit každý uživatel, který může ověřit pomocí jednotného přihlašování."**</span><span class="sxs-lookup"><span data-stu-id="9682c-195">Select **“Create any user who can authenticate with SSO.”**</span></span>

    <span data-ttu-id="9682c-196">b.</span><span class="sxs-lookup"><span data-stu-id="9682c-196">b.</span></span> <span data-ttu-id="9682c-197">**Poskytovatel služeb saml**: Vložit hodnotu ***urn: oasis: názvy: tc: SAML:2.0:nameid-formátu: trvalé***</span><span class="sxs-lookup"><span data-stu-id="9682c-197">**Service Provider saml**: Paste the value ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***</span></span>

    <span data-ttu-id="9682c-198">c.</span><span class="sxs-lookup"><span data-stu-id="9682c-198">c.</span></span> <span data-ttu-id="9682c-199">**Název pole zprostředkovatele Identity e-mailu v odpovědi ACS**: Vložit hodnotu`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="9682c-199">**Name of Identity Provider email field in ACS response**: Paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>

    <span data-ttu-id="9682c-200">d.</span><span class="sxs-lookup"><span data-stu-id="9682c-200">d.</span></span> <span data-ttu-id="9682c-201">**ID EntityDescriptor Entity zprostředkovatele identity:** vložení **SAML Entity ID** hodnota zkopírována z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9682c-201">**Identity Provider EntityDescriptor Entity ID:** Paste the **SAML Entity ID** value copied from the Azure portal.</span></span>

    <span data-ttu-id="9682c-202">e.</span><span class="sxs-lookup"><span data-stu-id="9682c-202">e.</span></span> <span data-ttu-id="9682c-203">**Adresa URL SingleSignOnService zprostředkovatele identity:** vložení **SAML jeden přihlašování adresa URL služby** z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="9682c-203">**Identity Provider SingleSignOnService URL:** Paste the **SAML Single Sign-On Service URL** from the Azure portal.</span></span>

    <span data-ttu-id="9682c-204">f.</span><span class="sxs-lookup"><span data-stu-id="9682c-204">f.</span></span> <span data-ttu-id="9682c-205">**Certifikát identity poskytovatele veřejných X509:** otevřete X509 certifikát stáhne z Azure v poznámkovém bloku a vložit obsah v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="9682c-205">**Identity Provider public X509 certificate:** Open the X509 certificate downloaded from the Azure in notepad and paste the contents in this box.</span></span> <span data-ttu-id="9682c-206">Zajistěte, že žádná zalomení uprostřed obsahu certifikátu.</span><span class="sxs-lookup"><span data-stu-id="9682c-206">Ensure there are no line breaks in the middle of the certificate contents.</span></span>
    
    <span data-ttu-id="9682c-207">g.</span><span class="sxs-lookup"><span data-stu-id="9682c-207">g.</span></span> <span data-ttu-id="9682c-208">Zkontrolujte následující políček: **povoleno, šifrování NameID a AuthnRequests přihlášení.**</span><span class="sxs-lookup"><span data-stu-id="9682c-208">Check the following checkboxes: **Enabled, Encrypt NameID and Sign AuthnRequests.**</span></span>

    <span data-ttu-id="9682c-209">h.</span><span class="sxs-lookup"><span data-stu-id="9682c-209">h.</span></span> <span data-ttu-id="9682c-210">Klikněte na tlačítko **nastavení jednotného přihlašování k aktualizaci** uložte nastavení.</span><span class="sxs-lookup"><span data-stu-id="9682c-210">Click **Update SSO Settings** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="9682c-211">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9682c-211">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9682c-212">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9682c-212">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9682c-213">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9682c-213">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9682c-214">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9682c-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="9682c-215">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9682c-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9682c-217">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9682c-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9682c-218">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9682c-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9682c-220">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9682c-220">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9682c-222">V horní části okna klikněte na položku **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9682c-222">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9682c-224">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9682c-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9682c-226">a.</span><span class="sxs-lookup"><span data-stu-id="9682c-226">a.</span></span> <span data-ttu-id="9682c-227">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9682c-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9682c-228">b.</span><span class="sxs-lookup"><span data-stu-id="9682c-228">b.</span></span> <span data-ttu-id="9682c-229">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9682c-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9682c-230">c.</span><span class="sxs-lookup"><span data-stu-id="9682c-230">c.</span></span> <span data-ttu-id="9682c-231">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9682c-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9682c-232">d.</span><span class="sxs-lookup"><span data-stu-id="9682c-232">d.</span></span> <span data-ttu-id="9682c-233">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9682c-233">Click **Create**.</span></span>
 
### <a name="creating-a-scalex-enterprise-test-user"></a><span data-ttu-id="9682c-234">Vytvoření zkušebního uživatele ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="9682c-234">Creating a ScaleX Enterprise test user</span></span>

<span data-ttu-id="9682c-235">Pokud chcete povolit uživatelům Azure AD přihlášení do firemní sítě ScaleX, se musí být zřízená v do firemní sítě ScaleX.</span><span class="sxs-lookup"><span data-stu-id="9682c-235">To enable Azure AD users to log in to ScaleX Enterprise, they must be provisioned in to ScaleX Enterprise.</span></span> <span data-ttu-id="9682c-236">V případě ScaleX Enterprise zřizování je automatická úloha a nejsou potřeba žádné ruční kroky.</span><span class="sxs-lookup"><span data-stu-id="9682c-236">In the case of ScaleX Enterprise, provisioning is an automatic task and no manual steps are required.</span></span> <span data-ttu-id="9682c-237">Každý uživatel, který může úspěšně ověřit pomocí přihlašovacích údajů jednotné přihlašování se automaticky zřídí na straně ScaleX.</span><span class="sxs-lookup"><span data-stu-id="9682c-237">Any user who can successfully authenticate with SSO credentials will be automatically provisioned on the ScaleX side.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9682c-238">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9682c-238">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9682c-239">V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup uživatelů k ScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="9682c-239">In this section, you enable Britta Simon to use Azure single sign-on by granting user access to ScaleX Enterprise.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9682c-241">**Pokud chcete přiřadit Britta Simon ScaleX Enterprise, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9682c-241">**To assign Britta Simon to ScaleX Enterprise, perform the following steps:**</span></span>

1. <span data-ttu-id="9682c-242">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9682c-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9682c-244">V seznamu aplikací vyberte **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="9682c-244">In the applications list, select **ScaleX Enterprise**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. <span data-ttu-id="9682c-246">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9682c-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9682c-248">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9682c-248">Click **Add** button.</span></span> <span data-ttu-id="9682c-249">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9682c-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9682c-251">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9682c-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9682c-252">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9682c-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9682c-253">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9682c-253">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="9682c-254">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9682c-254">Testing single sign-on</span></span>

<span data-ttu-id="9682c-255">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="9682c-255">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9682c-256">Klikněte na dlaždici ScaleX Enterprise na přístupovém panelu, můžete se získat automaticky přihlášení k aplikaci ScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="9682c-256">Click the ScaleX Enterprise tile in the Access Panel, you will get automatically signed-on to your ScaleX Enterprise application.</span></span> <span data-ttu-id="9682c-257">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="9682c-257">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9682c-258">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9682c-258">Additional resources</span></span>

* [<span data-ttu-id="9682c-259">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9682c-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9682c-260">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9682c-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

