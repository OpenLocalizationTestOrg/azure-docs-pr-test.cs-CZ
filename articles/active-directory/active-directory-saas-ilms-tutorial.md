---
title: 'Kurz: Azure Active Directory integrace s iLMS | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a iLMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 22c72020200138e78835ed7dd2661f18b824c785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a><span data-ttu-id="d9122-103">Kurz: Azure Active Directory integrace s iLMS</span><span class="sxs-lookup"><span data-stu-id="d9122-103">Tutorial: Azure Active Directory integration with iLMS</span></span>

<span data-ttu-id="d9122-104">V tomto kurzu zjistěte, jak integrovat iLMS s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d9122-104">In this tutorial, you learn how to integrate iLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d9122-105">Integrace iLMS s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d9122-105">Integrating iLMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d9122-106">Můžete řídit ve službě Azure AD, který má přístup k iLMS</span><span class="sxs-lookup"><span data-stu-id="d9122-106">You can control in Azure AD who has access to iLMS</span></span>
- <span data-ttu-id="d9122-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k iLMS (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9122-107">You can enable your users to automatically get signed-on to iLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d9122-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d9122-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d9122-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d9122-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9122-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d9122-110">Prerequisites</span></span>

<span data-ttu-id="d9122-111">Konfigurace integrace Azure AD s iLMS, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="d9122-111">To configure Azure AD integration with iLMS, you need the following items:</span></span>

- <span data-ttu-id="d9122-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9122-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d9122-113">ILMS jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="d9122-113">An iLMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d9122-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9122-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d9122-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="d9122-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d9122-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="d9122-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="d9122-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d9122-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d9122-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="d9122-118">Scenario description</span></span>
<span data-ttu-id="d9122-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d9122-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d9122-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="d9122-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d9122-121">Přidání iLMS z Galerie</span><span class="sxs-lookup"><span data-stu-id="d9122-121">Adding iLMS from the gallery</span></span>
2. <span data-ttu-id="d9122-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9122-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ilms-from-the-gallery"></a><span data-ttu-id="d9122-123">Přidání iLMS z Galerie</span><span class="sxs-lookup"><span data-stu-id="d9122-123">Adding iLMS from the gallery</span></span>
<span data-ttu-id="d9122-124">Při konfiguraci integrace iLMS do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS iLMS z galerie.</span><span class="sxs-lookup"><span data-stu-id="d9122-124">To configure the integration of iLMS into Azure AD, you need to add iLMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d9122-125">**Pokud chcete přidat iLMS z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9122-125">**To add iLMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d9122-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d9122-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d9122-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="d9122-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d9122-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d9122-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="d9122-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9122-131">To add new application, click **New application** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="d9122-133">Do vyhledávacího pole zadejte **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="d9122-133">In the search box, type **iLMS**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. <span data-ttu-id="d9122-135">Na panelu výsledků vyberte **iLMS**, pak klikněte na tlačítko **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d9122-135">In the results panel, select **iLMS**, then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d9122-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9122-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d9122-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s iLMS podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="d9122-138">In this section, you configure and test Azure AD single sign-on with iLMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d9122-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v iLMS je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d9122-139">For single sign-on to work, Azure AD needs to know what the counterpart user in iLMS is to a user in Azure AD.</span></span> <span data-ttu-id="d9122-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v iLMS musí navázat.</span><span class="sxs-lookup"><span data-stu-id="d9122-140">In other words, a link relationship between an Azure AD user and the related user in iLMS needs to be established.</span></span>

<span data-ttu-id="d9122-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v iLMS.</span><span class="sxs-lookup"><span data-stu-id="d9122-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in iLMS.</span></span>

<span data-ttu-id="d9122-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s iLMS, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="d9122-142">To configure and test Azure AD single sign-on with iLMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d9122-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="d9122-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d9122-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9122-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d9122-145">**[Vytváření testovacího uživatele iLMS](#creating-an-ilms-test-user)**  – Pokud chcete mít protějšek Britta Simon v iLMS propojeném s Azure AD reprezentace jí.</span><span class="sxs-lookup"><span data-stu-id="d9122-145">**[Creating an iLMS test user](#creating-an-ilms-test-user)** - to have a counterpart of Britta Simon in iLMS that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="d9122-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9122-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d9122-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d9122-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d9122-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9122-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d9122-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci iLMS.</span><span class="sxs-lookup"><span data-stu-id="d9122-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your iLMS application.</span></span>

<span data-ttu-id="d9122-150">**Ke konfiguraci Azure AD jednotné přihlašování s iLMS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9122-150">**To configure Azure AD single sign-on with iLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="d9122-151">Na portálu Azure na **iLMS** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="d9122-151">In the Azure portal, on the **iLMS** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="d9122-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9122-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. <span data-ttu-id="d9122-155">Na **iLMS domény a adresy URL** část, proveďte následující kroky, pokud chcete nakonfigurovat aplikace **IDP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d9122-155">On the **iLMS Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    <span data-ttu-id="d9122-157">a.</span><span class="sxs-lookup"><span data-stu-id="d9122-157">a.</span></span> <span data-ttu-id="d9122-158">V **identifikátor** textovému poli, Vložit **identifikátor** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení iLMS správce portálu.</span><span class="sxs-lookup"><span data-stu-id="d9122-158">In the **Identifier** textbox, paste the **Identifier** value you copy from **Service Provider** section of SAML settings in iLMS admin portal.</span></span>

    <span data-ttu-id="d9122-159">b.</span><span class="sxs-lookup"><span data-stu-id="d9122-159">b.</span></span> <span data-ttu-id="d9122-160">V **adresa URL odpovědi** textovému poli, Vložit **koncový bod (URL)** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení v portálu pro správu iLMS následující opakování`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="d9122-160">In the **Reply URL** textbox, paste the **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal having the following pattern `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>

    >[!Note]
    ><span data-ttu-id="d9122-161">Tato 123456 je příkladem hodnoty identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="d9122-161">This '123456' is an example value of identifier.</span></span>

4. <span data-ttu-id="d9122-162">Zkontrolujte **zobrazit upřesňující nastavení adresy URL**, pokud chcete nakonfigurovat aplikace **SP** iniciované režimu:</span><span class="sxs-lookup"><span data-stu-id="d9122-162">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    <span data-ttu-id="d9122-164">V **přihlašovací adresa URL** textovému poli, Vložit **koncový bod (URL)** hodnotu zkopírujete z **poskytovatele služeb** části SAML nastavení v portálu pro správu iLMS jako`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="d9122-164">In the **Sign-on URL** textbox, paste the **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal as `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>     

5. <span data-ttu-id="d9122-165">Pokud chcete povolit zřizování JIT, očekává iLMS aplikace SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="d9122-165">To enable JIT provisioning, iLMS application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="d9122-166">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d9122-166">Configure the following claims for this application.</span></span> <span data-ttu-id="d9122-167">Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9122-167">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="d9122-168">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="d9122-168">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/4.png)
    
    <span data-ttu-id="d9122-170">Vytvoření **oddělení, oblast** a **dělení** atributy a přidejte název těchto atributů v iLMS.</span><span class="sxs-lookup"><span data-stu-id="d9122-170">Create **Department, Region** and **Division** attributes and add the name of these attributes in iLMS.</span></span> <span data-ttu-id="d9122-171">Všechny tyto atributy, které jsou uvedené výše se vyžadují.</span><span class="sxs-lookup"><span data-stu-id="d9122-171">All these attributes shown above are required.</span></span>  

    > [!NOTE] 
    > <span data-ttu-id="d9122-172">Je třeba povolit **vytvořit uživatelský účet Un-recognized** v iLMS k mapování těchto atributů.</span><span class="sxs-lookup"><span data-stu-id="d9122-172">You have to enable **Create Un-recognized User Account** in iLMS to map these attributes.</span></span> <span data-ttu-id="d9122-173">Postupujte podle pokynů [zde](http://support.inspiredelearning.com/customer/portal/articles/2204526) , kde získáte představu na konfiguraci atributy.</span><span class="sxs-lookup"><span data-stu-id="d9122-173">Follow the instructions [here](http://support.inspiredelearning.com/customer/portal/articles/2204526) to get an idea on the attributes configuration.</span></span>

6. <span data-ttu-id="d9122-174">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9122-174">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="d9122-175">Název atributu</span><span class="sxs-lookup"><span data-stu-id="d9122-175">Attribute Name</span></span> | <span data-ttu-id="d9122-176">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="d9122-176">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="d9122-177">dělení</span><span class="sxs-lookup"><span data-stu-id="d9122-177">division</span></span> | <span data-ttu-id="d9122-178">User.Department</span><span class="sxs-lookup"><span data-stu-id="d9122-178">user.department</span></span> |
    | <span data-ttu-id="d9122-179">Oblast</span><span class="sxs-lookup"><span data-stu-id="d9122-179">region</span></span> | <span data-ttu-id="d9122-180">User.state</span><span class="sxs-lookup"><span data-stu-id="d9122-180">user.state</span></span> |
    | <span data-ttu-id="d9122-181">Oddělení</span><span class="sxs-lookup"><span data-stu-id="d9122-181">department</span></span> | <span data-ttu-id="d9122-182">User.jobtitle</span><span class="sxs-lookup"><span data-stu-id="d9122-182">user.jobtitle</span></span> |

    <span data-ttu-id="d9122-183">a.</span><span class="sxs-lookup"><span data-stu-id="d9122-183">a.</span></span> <span data-ttu-id="d9122-184">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9122-184">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    <span data-ttu-id="d9122-187">b.</span><span class="sxs-lookup"><span data-stu-id="d9122-187">b.</span></span> <span data-ttu-id="d9122-188">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="d9122-188">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="d9122-189">c.</span><span class="sxs-lookup"><span data-stu-id="d9122-189">c.</span></span> <span data-ttu-id="d9122-190">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="d9122-190">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="d9122-191">d.</span><span class="sxs-lookup"><span data-stu-id="d9122-191">d.</span></span> <span data-ttu-id="d9122-192">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="d9122-192">Click **Ok**</span></span>

7. <span data-ttu-id="d9122-193">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="d9122-193">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. <span data-ttu-id="d9122-195">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d9122-195">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="d9122-197">V okně prohlížeče jiný web, přihlaste se k vaší **portál pro správu iLMS** jako správce.</span><span class="sxs-lookup"><span data-stu-id="d9122-197">In a different web browser window, log in to your **iLMS admin portal** as an administrator.</span></span>

10. <span data-ttu-id="d9122-198">Klikněte na tlačítko **SSO:SAML** pod **nastavení** otevřete SAML nastavení a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9122-198">Click **SSO:SAML** under **Settings** tab to open SAML settings and perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/1.png) 

    <span data-ttu-id="d9122-200">a.</span><span class="sxs-lookup"><span data-stu-id="d9122-200">a.</span></span> <span data-ttu-id="d9122-201">Rozbalte **poskytovatele služeb** části a zkopírujte **identifikátor** a **koncový bod (URL)** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d9122-201">Expand the **Service Provider** section and copy the **Identifier** and **Endpoint (URL)** value.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/2.png) 

    <span data-ttu-id="d9122-203">b.</span><span class="sxs-lookup"><span data-stu-id="d9122-203">b.</span></span> <span data-ttu-id="d9122-204">V části **zprostředkovatele Identity** klikněte na tlačítko **importovat Metadata**.</span><span class="sxs-lookup"><span data-stu-id="d9122-204">Under **Identity Provider** section, click **Import Metadata**.</span></span>
    
    <span data-ttu-id="d9122-205">c.</span><span class="sxs-lookup"><span data-stu-id="d9122-205">c.</span></span> <span data-ttu-id="d9122-206">Vyberte **Metadata** soubor stažený z portálu Azure z **SAML podpisový certifikát** části.</span><span class="sxs-lookup"><span data-stu-id="d9122-206">Select the **Metadata** file downloaded from Azure Portal from **SAML Signing Certificate** section.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    <span data-ttu-id="d9122-208">d.</span><span class="sxs-lookup"><span data-stu-id="d9122-208">d.</span></span> <span data-ttu-id="d9122-209">Pokud chcete povolit zřizování můžete vytvořit účty iLMS pro zrušení JIT-rozpoznat uživatele, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d9122-209">If you want to enable JIT provisioning to create iLMS accounts for un-recognize users, follow below steps:</span></span>
        
       - <span data-ttu-id="d9122-210">Zkontrolujte **vytvořte účet bez rozpoznaný uživatel**.</span><span class="sxs-lookup"><span data-stu-id="d9122-210">Check **Create Un-recognized User Account**.</span></span>
       
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  <span data-ttu-id="d9122-212">Mapování atributů ve službě Azure AD s atributy v iLMS.</span><span class="sxs-lookup"><span data-stu-id="d9122-212">Map the attributes in Azure AD with the attributes in iLMS.</span></span> <span data-ttu-id="d9122-213">Ve sloupci atributu zadejte atributy název nebo výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d9122-213">In the attribute column, specify the attributes name or the default value.</span></span>

    <span data-ttu-id="d9122-214">e.</span><span class="sxs-lookup"><span data-stu-id="d9122-214">e.</span></span> <span data-ttu-id="d9122-215">Přejděte na **obchodní pravidla** kartě a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9122-215">Go to **Business Rules** tab and perform the following steps:</span></span> 
        
       ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/5.png)

       - <span data-ttu-id="d9122-217">Zkontrolujte **vytvořit Un-recognized oblastí, rozdělení a oddělení** vytvořit oblastí, rozdělení a oddělení, které už neexistují v době jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9122-217">Check **Create Un-recognized Regions, Divisions and Departments** to create Regions, Divisions, and Departments that do not already exist at the time of Single Sign-on.</span></span>
        
       - <span data-ttu-id="d9122-218">Zkontrolujte **aktualizace uživatelského profilu při přihlášení** k určení, zda je aktualizovat profil uživatele s každou jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9122-218">Check **Update User Profile During Sign-in** to specify whether the user’s profile is updated with each Single Sign-on.</span></span> 
        
       - <span data-ttu-id="d9122-219">Pokud **"Aktualizace prázdné hodnoty pro jiný povinného pole v profilu uživatele"** zaškrtnutá možnost, volitelné profil pole, která jsou prázdné po přihlášení bude také způsobit iLMS profil uživatele obsahuje prázdné hodnoty těchto polí.</span><span class="sxs-lookup"><span data-stu-id="d9122-219">If the **“Update Blank Values for Non Mandatory Fields in User Profile”** option is checked, optional profile fields that are blank upon sign in will also cause the user’s iLMS profile to contain blank values for those fields.</span></span>
        
       - <span data-ttu-id="d9122-220">Zkontrolujte **odeslat E-mail s oznámením chyba** a zadejte e-mailu uživatele, kde chcete dostávat e-mailové oznámení chyby.</span><span class="sxs-lookup"><span data-stu-id="d9122-220">Check **Send Error Notification Email** and enter the email of the user where you want to receive the error notification email.</span></span>

11. <span data-ttu-id="d9122-221">Klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení.</span><span class="sxs-lookup"><span data-stu-id="d9122-221">Click **Save** button to save the settings.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="d9122-223">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="d9122-223">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d9122-224">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="d9122-224">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d9122-225">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d9122-225">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
    
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d9122-226">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9122-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="d9122-227">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9122-227">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="d9122-229">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9122-229">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d9122-230">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="d9122-230">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d9122-232">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d9122-232">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d9122-234">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9122-234">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d9122-236">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d9122-236">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d9122-238">a.</span><span class="sxs-lookup"><span data-stu-id="d9122-238">a.</span></span> <span data-ttu-id="d9122-239">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d9122-239">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d9122-240">b.</span><span class="sxs-lookup"><span data-stu-id="d9122-240">b.</span></span> <span data-ttu-id="d9122-241">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d9122-241">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d9122-242">c.</span><span class="sxs-lookup"><span data-stu-id="d9122-242">c.</span></span> <span data-ttu-id="d9122-243">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="d9122-243">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d9122-244">d.</span><span class="sxs-lookup"><span data-stu-id="d9122-244">d.</span></span> <span data-ttu-id="d9122-245">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d9122-245">Click **Create**.</span></span>
 
### <a name="creating-an-ilms-test-user"></a><span data-ttu-id="d9122-246">Vytváření testovacího uživatele iLMS</span><span class="sxs-lookup"><span data-stu-id="d9122-246">Creating an iLMS test user</span></span>

<span data-ttu-id="d9122-247">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatelé jsou automaticky vytvořené v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d9122-247">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="d9122-248">JIT bude fungovat, pokud jste klikli **vytvořit uživatelský účet Un-recognized** políčko během SAML nastavení konfigurace na portál pro správu iLMS.</span><span class="sxs-lookup"><span data-stu-id="d9122-248">JIT will work, if you have clicked the **Create Un-recognized User Account** checkbox during SAML configuration setting at iLMS admin portal.</span></span>

<span data-ttu-id="d9122-249">Pokud potřebujete vytvořit uživatele s ručně, postupujte podle následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="d9122-249">If you need to create an user manually, then follow below steps :</span></span>

1. <span data-ttu-id="d9122-250">Přihlaste se k serveru vaší společnosti iLMS jako správce.</span><span class="sxs-lookup"><span data-stu-id="d9122-250">Log in to your iLMS company site as an administrator.</span></span>

2. <span data-ttu-id="d9122-251">Klikněte na tlačítko **"Uživatel zaregistrovat"** pod **uživatelé** otevřete **registrovat uživatele** stránky.</span><span class="sxs-lookup"><span data-stu-id="d9122-251">Click **“Register User”** under **Users** tab to open **Register User** page.</span></span> 
   
   ![Můžete přidat zaměstnance](./media/active-directory-saas-ilms-tutorial/3.png)

3. <span data-ttu-id="d9122-253">Na **"Registrovat uživatele"** proveďte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="d9122-253">On the **“Register User”** page, perform the following steps.</span></span>

    ![Můžete přidat zaměstnance](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    <span data-ttu-id="d9122-255">a.</span><span class="sxs-lookup"><span data-stu-id="d9122-255">a.</span></span> <span data-ttu-id="d9122-256">V **křestní jméno** textovému poli, název typu první Britta.</span><span class="sxs-lookup"><span data-stu-id="d9122-256">In the **First Name** textbox, type the first name Britta.</span></span>
   
    <span data-ttu-id="d9122-257">b.</span><span class="sxs-lookup"><span data-stu-id="d9122-257">b.</span></span> <span data-ttu-id="d9122-258">V **příjmení** textovému poli, zadejte příjmení Simon.</span><span class="sxs-lookup"><span data-stu-id="d9122-258">In the **Last Name** textbox, type the last name Simon.</span></span>

    <span data-ttu-id="d9122-259">c.</span><span class="sxs-lookup"><span data-stu-id="d9122-259">c.</span></span> <span data-ttu-id="d9122-260">V **ID e-mailu** textovému poli, zadejte e-mailovou adresu účtu Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d9122-260">In the **Email ID** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="d9122-261">d.</span><span class="sxs-lookup"><span data-stu-id="d9122-261">d.</span></span> <span data-ttu-id="d9122-262">V **oblast** rozevíracího seznamu, vyberte hodnotu pro oblast.</span><span class="sxs-lookup"><span data-stu-id="d9122-262">In the **Region** dropdown, select the value for region.</span></span>

    <span data-ttu-id="d9122-263">e.</span><span class="sxs-lookup"><span data-stu-id="d9122-263">e.</span></span> <span data-ttu-id="d9122-264">V **dělení** rozevíracího seznamu, vyberte hodnotu pro dělení.</span><span class="sxs-lookup"><span data-stu-id="d9122-264">In the **Division** dropdown, select the value for division.</span></span>

    <span data-ttu-id="d9122-265">f.</span><span class="sxs-lookup"><span data-stu-id="d9122-265">f.</span></span> <span data-ttu-id="d9122-266">V **oddělení** rozevíracího seznamu, vyberte hodnotu pro oddělení.</span><span class="sxs-lookup"><span data-stu-id="d9122-266">In the **Department** dropdown, select the value for department.</span></span>

    <span data-ttu-id="d9122-267">g.</span><span class="sxs-lookup"><span data-stu-id="d9122-267">g.</span></span> <span data-ttu-id="d9122-268">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="d9122-268">Click **Save**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d9122-269">Můžete odeslat e-mailu registrace uživatele tak, že vyberete **odesílání pošty registrace** zaškrtávací políčko.</span><span class="sxs-lookup"><span data-stu-id="d9122-269">You can send registration mail to user by selecting **Send Registration Mail** checkbox.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d9122-270">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="d9122-270">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d9122-271">V této části povolíte Britta Simon používat tak, že udělíte přístup k iLMS Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="d9122-271">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to iLMS.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="d9122-273">**Pokud chcete přiřadit Britta Simon iLMS, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="d9122-273">**To assign Britta Simon to iLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="d9122-274">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="d9122-274">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="d9122-276">V seznamu aplikací vyberte **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="d9122-276">In the applications list, select **iLMS**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. <span data-ttu-id="d9122-278">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="d9122-278">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="d9122-280">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d9122-280">Click **Add** button.</span></span> <span data-ttu-id="d9122-281">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9122-281">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="d9122-283">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="d9122-283">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d9122-284">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9122-284">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d9122-285">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="d9122-285">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d9122-286">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="d9122-286">Testing single sign-on</span></span>

<span data-ttu-id="d9122-287">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="d9122-287">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d9122-288">Když kliknete na dlaždici iLMS na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci iLMS.</span><span class="sxs-lookup"><span data-stu-id="d9122-288">When you click the iLMS tile in the Access Panel, you should get automatically signed-on to your iLMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9122-289">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d9122-289">Additional resources</span></span>

* [<span data-ttu-id="d9122-290">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d9122-290">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d9122-291">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d9122-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

