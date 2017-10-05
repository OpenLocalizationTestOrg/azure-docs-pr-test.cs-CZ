---
title: "Kurz: Azure Active Directory integrace s virtuálními počítači IQNavigator | Microsoft Docs"
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a IQNavigator virtuálních počítačů."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a8a09b25-dfa5-4c31-aea2-53bf1853b365
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 1164723a171843541098f6adbda0e65f7e82a0cb
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-iqnavigator-vms"></a><span data-ttu-id="51e5a-103">Kurz: Azure Active Directory integrace s virtuálními počítači IQNavigator</span><span class="sxs-lookup"><span data-stu-id="51e5a-103">Tutorial: Azure Active Directory integration with IQNavigator VMS</span></span>

<span data-ttu-id="51e5a-104">V tomto kurzu zjistěte, jak integrovat IQNavigator virtuální počítače v Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="51e5a-104">In this tutorial, you learn how to integrate IQNavigator VMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="51e5a-105">Integrace IQNavigator virtuálních počítačů s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="51e5a-105">Integrating IQNavigator VMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="51e5a-106">Můžete řídit ve službě Azure AD, který má přístup k virtuálním počítačům IQNavigator</span><span class="sxs-lookup"><span data-stu-id="51e5a-106">You can control in Azure AD who has access to IQNavigator VMS</span></span>
- <span data-ttu-id="51e5a-107">Můžete povolit uživatelům, aby automaticky získat přihlášeného k virtuálním počítačům IQNavigator (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="51e5a-107">You can enable your users to automatically get signed-on to IQNavigator VMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="51e5a-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="51e5a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="51e5a-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="51e5a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51e5a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="51e5a-110">Prerequisites</span></span>

<span data-ttu-id="51e5a-111">Ke konfiguraci integrace služby Azure AD s virtuálními počítači IQNavigator, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="51e5a-111">To configure Azure AD integration with IQNavigator VMS, you need the following items:</span></span>

- <span data-ttu-id="51e5a-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="51e5a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="51e5a-113">Virtuální počítače IQNavigator jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="51e5a-113">A IQNavigator VMS single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="51e5a-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="51e5a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="51e5a-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="51e5a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="51e5a-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="51e5a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="51e5a-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat a jeden měsíc zkušební [nabídka zkušební verze](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51e5a-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="51e5a-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="51e5a-118">Scenario description</span></span>
<span data-ttu-id="51e5a-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="51e5a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="51e5a-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="51e5a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="51e5a-121">Přidání IQNavigator virtuálních počítačů z Galerie</span><span class="sxs-lookup"><span data-stu-id="51e5a-121">Adding IQNavigator VMS from the gallery</span></span>
2. <span data-ttu-id="51e5a-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="51e5a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-iqnavigator-vms-from-the-gallery"></a><span data-ttu-id="51e5a-123">Přidání IQNavigator virtuálních počítačů z Galerie</span><span class="sxs-lookup"><span data-stu-id="51e5a-123">Adding IQNavigator VMS from the gallery</span></span>
<span data-ttu-id="51e5a-124">Při konfiguraci integrace IQNavigator virtuálních počítačů do Azure AD, potřebujete přidat IQNavigator virtuálních počítačů z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="51e5a-124">To configure the integration of IQNavigator VMS into Azure AD, you need to add IQNavigator VMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="51e5a-125">**Pokud chcete přidat virtuální počítače IQNavigator z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="51e5a-125">**To add IQNavigator VMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="51e5a-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="51e5a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="51e5a-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="51e5a-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="51e5a-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="51e5a-133">Do vyhledávacího pole zadejte **virtuální počítače IQNavigator**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-133">In the search box, type **IQNavigator VMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_search.png)

5. <span data-ttu-id="51e5a-135">Na panelu výsledků vyberte **virtuální počítače IQNavigator**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="51e5a-135">In the results panel, select **IQNavigator VMS**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="51e5a-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="51e5a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="51e5a-138">V této části můžete nakonfigurovat, testovací Azure AD jednotné přihlašování s virtuálními počítači IQNavigator podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="51e5a-138">In this section, you configure and test Azure AD single sign-on with IQNavigator VMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="51e5a-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějšku ve virtuálních počítačích IQNavigator je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="51e5a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in IQNavigator VMS is to a user in Azure AD.</span></span> <span data-ttu-id="51e5a-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské ve virtuálních počítačích IQNavigator musí navázat.</span><span class="sxs-lookup"><span data-stu-id="51e5a-140">In other words, a link relationship between an Azure AD user and the related user in IQNavigator VMS needs to be established.</span></span>

<span data-ttu-id="51e5a-141">Ve virtuálních počítačích IQNavigator, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="51e5a-141">In IQNavigator VMS, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="51e5a-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s virtuálními počítači IQNavigator, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="51e5a-142">To configure and test Azure AD single sign-on with IQNavigator VMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="51e5a-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="51e5a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="51e5a-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="51e5a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="51e5a-145">**[Vytvoření zkušebního uživatele virtuální počítače IQNavigator](#creating-a-iqnavigator-vms-test-user)**  – Pokud chcete mít protějšek Britta Simon ve virtuálních počítačích IQNavigator propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="51e5a-145">**[Creating a IQNavigator VMS test user](#creating-a-iqnavigator-vms-test-user)** - to have a counterpart of Britta Simon in IQNavigator VMS that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="51e5a-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51e5a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="51e5a-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="51e5a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="51e5a-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="51e5a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="51e5a-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci IQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51e5a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your IQNavigator VMS application.</span></span>

<span data-ttu-id="51e5a-150">**Ke konfiguraci Azure AD jednotné přihlašování s virtuálními počítači IQNavigator, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="51e5a-150">**To configure Azure AD single sign-on with IQNavigator VMS, perform the following steps:**</span></span>

1. <span data-ttu-id="51e5a-151">Na portálu Azure na **virtuální počítače IQNavigator** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-151">In the Azure portal, on the **IQNavigator VMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="51e5a-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51e5a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_samlbase.png)

3. <span data-ttu-id="51e5a-155">Na **IQNavigator virtuálních počítačů domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="51e5a-155">On the **IQNavigator VMS Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url.png)

    <span data-ttu-id="51e5a-157">a.</span><span class="sxs-lookup"><span data-stu-id="51e5a-157">a.</span></span> <span data-ttu-id="51e5a-158">V **identifikátor** textovému poli, zadejte adresu URL:`iqn.com`</span><span class="sxs-lookup"><span data-stu-id="51e5a-158">In the **Identifier** textbox, type the URL:`iqn.com`</span></span>

    <span data-ttu-id="51e5a-159">b.</span><span class="sxs-lookup"><span data-stu-id="51e5a-159">b.</span></span> <span data-ttu-id="51e5a-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="51e5a-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.iqnavigator.com/security/login?client_name=https://sts.window.net/<instance name>`</span></span>

4. <span data-ttu-id="51e5a-161">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="51e5a-161">Check **Show advanced URL settings**, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_url1.png)

    <span data-ttu-id="51e5a-163">V **předávání stavu** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<subdomain>.iqnavigator.com`</span><span class="sxs-lookup"><span data-stu-id="51e5a-163">In the **Relay state** textbox, type a URL using the following pattern:`https://<subdomain>.iqnavigator.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="51e5a-164">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="51e5a-164">These values are not real.</span></span> <span data-ttu-id="51e5a-165">Tyto hodnoty aktualizujte se skutečným stavem adresa URL odpovědi a předávání.</span><span class="sxs-lookup"><span data-stu-id="51e5a-165">Update these values with the actual Reply URL and Relay state.</span></span> <span data-ttu-id="51e5a-166">Obraťte se na [tým podpory IQNavigator virtuální počítače klientů](https://www.beeline.com/iqn-product-support/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="51e5a-166">Contact [IQNavigator VMS Client support team](https://www.beeline.com/iqn-product-support/) to get these values.</span></span> 

5. <span data-ttu-id="51e5a-167">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51e5a-167">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="51e5a-169">Ke generování **Metadata** adresu url, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="51e5a-169">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="51e5a-170">a.</span><span class="sxs-lookup"><span data-stu-id="51e5a-170">a.</span></span> <span data-ttu-id="51e5a-171">Klikněte na tlačítko **registrace aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-171">Click **App registrations**.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appregistrations.png)
   
    <span data-ttu-id="51e5a-173">b.</span><span class="sxs-lookup"><span data-stu-id="51e5a-173">b.</span></span> <span data-ttu-id="51e5a-174">Klikněte na tlačítko **koncové body** otevřete **koncové body** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-174">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpointicon.png)

    <span data-ttu-id="51e5a-176">c.</span><span class="sxs-lookup"><span data-stu-id="51e5a-176">c.</span></span> <span data-ttu-id="51e5a-177">Klikněte na tlačítko Kopírovat kopírování **dokument FEDERAČNÍCH METADAT** adresy url a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="51e5a-177">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_endpoint.png)
     
    <span data-ttu-id="51e5a-179">d.</span><span class="sxs-lookup"><span data-stu-id="51e5a-179">d.</span></span> <span data-ttu-id="51e5a-180">Nyní přejděte na stránku vlastností **virtuální počítače IQNavigator** a zkopírujte **Id aplikace** pomocí **kopie** tlačítko a vložte do poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="51e5a-180">Now go to the property page of **IQNavigator VMS** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_appid.png)

    <span data-ttu-id="51e5a-182">e.</span><span class="sxs-lookup"><span data-stu-id="51e5a-182">e.</span></span> <span data-ttu-id="51e5a-183">Vygenerovat **adresu URL metadat** pomocí následujícího vzorce:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="51e5a-183">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>

7. <span data-ttu-id="51e5a-184">Aplikace IQNavigator očekávají, že hodnota identifikátoru jedinečná uživatelská v názvu identifikátor deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="51e5a-184">IQNavigator application expect the unique user identifier value in the Name Identifier claim.</span></span> <span data-ttu-id="51e5a-185">Zákazníka můžete namapovat správné hodnoty pro název identifikátor deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="51e5a-185">Customer can map the correct value for the Name Identifier claim.</span></span> <span data-ttu-id="51e5a-186">V takovém případě jsme mají mapovat uživatele. UserPrincipalName pro ukázku účel.</span><span class="sxs-lookup"><span data-stu-id="51e5a-186">In this case we have mapped the user.UserPrincipalName for the demo purpose.</span></span> <span data-ttu-id="51e5a-187">Ale podle nastavení organizace by měla být mapována správnou hodnotu pro ni.</span><span class="sxs-lookup"><span data-stu-id="51e5a-187">But according to your organization settings you should map the correct value for it.</span></span>   

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_attribute.png)

8. <span data-ttu-id="51e5a-189">Na **konfigurace virtuálních počítačů IQNavigator** klikněte na tlačítko **konfigurace virtuálních počítačů IQNavigator** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-189">On the **IQNavigator VMS Configuration** section, click **Configure IQNavigator VMS** to open **Configure sign-on** window.</span></span> <span data-ttu-id="51e5a-190">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="51e5a-190">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_configure.png) 

9. <span data-ttu-id="51e5a-192">Konfigurace jednotného přihlašování na **IQNavigator virtuální počítače** straně, budete muset odeslat **adresu URL metadat**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [tým podpory pro virtuální počítače IQNavigator](https://www.beeline.com/iqn-product-support/).</span><span class="sxs-lookup"><span data-stu-id="51e5a-192">To configure single sign-on on **IQNavigator VMS** side, you need to send the **Metadata URL**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/).</span></span> <span data-ttu-id="51e5a-193">Nastavují toto nastavení tak, aby měl jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="51e5a-193">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="51e5a-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="51e5a-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="51e5a-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="51e5a-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="51e5a-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="51e5a-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="51e5a-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="51e5a-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="51e5a-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="51e5a-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="51e5a-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="51e5a-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="51e5a-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="51e5a-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="51e5a-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="51e5a-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="51e5a-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="51e5a-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-iqnavigatorvms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="51e5a-209">a.</span><span class="sxs-lookup"><span data-stu-id="51e5a-209">a.</span></span> <span data-ttu-id="51e5a-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="51e5a-211">b.</span><span class="sxs-lookup"><span data-stu-id="51e5a-211">b.</span></span> <span data-ttu-id="51e5a-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="51e5a-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="51e5a-213">c.</span><span class="sxs-lookup"><span data-stu-id="51e5a-213">c.</span></span> <span data-ttu-id="51e5a-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="51e5a-215">d.</span><span class="sxs-lookup"><span data-stu-id="51e5a-215">d.</span></span> <span data-ttu-id="51e5a-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-216">Click **Create**.</span></span>
 
### <a name="creating-a-iqnavigator-vms-test-user"></a><span data-ttu-id="51e5a-217">Vytvoření zkušebního uživatele IQNavigator virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="51e5a-217">Creating a IQNavigator VMS test user</span></span>

<span data-ttu-id="51e5a-218">Cílem této části je vytvoření uživatele volat Britta Simon ve virtuálních počítačích IQNavigator.</span><span class="sxs-lookup"><span data-stu-id="51e5a-218">The objective of this section is to create a user called Britta Simon in IQNavigator VMS.</span></span> <span data-ttu-id="51e5a-219">Práce s [tým podpory pro virtuální počítače IQNavigator](https://www.beeline.com/iqn-product-support/) přidat uživatele do účtu IQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51e5a-219">Work with  [IQNavigator VMS support team](https://www.beeline.com/iqn-product-support/) to add the users in the IQNavigator VMS account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="51e5a-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="51e5a-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="51e5a-221">V této části povolíte Britta Simon používat tak, že udělíte přístup k virtuálním počítačům IQNavigator Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="51e5a-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to IQNavigator VMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="51e5a-223">**Pokud chcete přiřadit Britta Simon IQNavigator virtuální počítače, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="51e5a-223">**To assign Britta Simon to IQNavigator VMS, perform the following steps:**</span></span>

1. <span data-ttu-id="51e5a-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="51e5a-226">V seznamu aplikací vyberte **virtuální počítače IQNavigator**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-226">In the applications list, select **IQNavigator VMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_iqnavigatorvms_app.png) 

3. <span data-ttu-id="51e5a-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="51e5a-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="51e5a-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="51e5a-230">Click **Add** button.</span></span> <span data-ttu-id="51e5a-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="51e5a-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="51e5a-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="51e5a-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="51e5a-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="51e5a-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="51e5a-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="51e5a-236">Testing single sign-on</span></span>

<span data-ttu-id="51e5a-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="51e5a-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="51e5a-238">Když kliknete na dlaždici IQNavigator virtuální počítače na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci IQNavigator virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="51e5a-238">When you click the IQNavigator VMS tile in the Access Panel, you should get automatically signed-on to your IQNavigator VMS application.</span></span>
<span data-ttu-id="51e5a-239">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="51e5a-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="51e5a-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="51e5a-240">Additional resources</span></span>

* [<span data-ttu-id="51e5a-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="51e5a-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="51e5a-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="51e5a-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-iqnavigatorvms-tutorial/tutorial_general_203.png

