---
title: 'Kurz: Azure Active Directory integrace s ServiceChannel | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ServiceChannel."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 7e1dad18ff0ae9a9102b789b2cb32e7b96ed3d38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a><span data-ttu-id="f848b-103">Kurz: Azure Active Directory integrace s ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="f848b-103">Tutorial: Azure Active Directory integration with ServiceChannel</span></span>

<span data-ttu-id="f848b-104">V tomto kurzu zjistěte, jak integrovat ServiceChannel s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f848b-104">In this tutorial, you learn how to integrate ServiceChannel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f848b-105">Integrace ServiceChannel s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f848b-105">Integrating ServiceChannel with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f848b-106">Můžete řídit ve službě Azure AD, který má přístup k ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="f848b-106">You can control in Azure AD who has access to ServiceChannel</span></span>
- <span data-ttu-id="f848b-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ServiceChannel (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f848b-107">You can enable your users to automatically get signed-on to ServiceChannel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f848b-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu pro správu Azure</span><span class="sxs-lookup"><span data-stu-id="f848b-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="f848b-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f848b-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f848b-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f848b-110">Prerequisites</span></span>

<span data-ttu-id="f848b-111">Konfigurace integrace Azure AD s ServiceChannel, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f848b-111">To configure Azure AD integration with ServiceChannel, you need the following items:</span></span>

- <span data-ttu-id="f848b-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f848b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f848b-113">ServiceChannel jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f848b-113">A ServiceChannel single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f848b-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f848b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f848b-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f848b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f848b-116">Provozním prostředí byste neměli používat, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="f848b-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="f848b-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f848b-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f848b-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f848b-118">Scenario description</span></span>
<span data-ttu-id="f848b-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f848b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f848b-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f848b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f848b-121">Přidání ServiceChannel z Galerie</span><span class="sxs-lookup"><span data-stu-id="f848b-121">Adding ServiceChannel from the gallery</span></span>
2. <span data-ttu-id="f848b-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f848b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-servicechannel-from-the-gallery"></a><span data-ttu-id="f848b-123">Přidání ServiceChannel z Galerie</span><span class="sxs-lookup"><span data-stu-id="f848b-123">Adding ServiceChannel from the gallery</span></span>
<span data-ttu-id="f848b-124">Při konfiguraci integrace ServiceChannel do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS ServiceChannel z galerie.</span><span class="sxs-lookup"><span data-stu-id="f848b-124">To configure the integration of ServiceChannel into Azure AD, you need to add ServiceChannel from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f848b-125">**Pokud chcete přidat ServiceChannel z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f848b-125">**To add ServiceChannel from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f848b-126">V  **[portálu pro správu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f848b-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f848b-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f848b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f848b-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f848b-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f848b-131">Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-131">Click **Add** button on the top of the dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f848b-133">Do vyhledávacího pole zadejte **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="f848b-133">In the search box, type **ServiceChannel**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. <span data-ttu-id="f848b-135">Na panelu výsledků vyberte **ServiceChannel**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f848b-135">In the results panel, select **ServiceChannel**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f848b-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f848b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f848b-138">V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s ServiceChannel podle testovacího uživatele názvem "Britta Simon".</span><span class="sxs-lookup"><span data-stu-id="f848b-138">In this section, you configure and test Azure AD single sign-on with ServiceChannel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f848b-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ServiceChannel je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f848b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ServiceChannel is to a user in Azure AD.</span></span> <span data-ttu-id="f848b-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ServiceChannel musí navázat.</span><span class="sxs-lookup"><span data-stu-id="f848b-140">In other words, a link relationship between an Azure AD user and the related user in ServiceChannel needs to be established.</span></span>

<span data-ttu-id="f848b-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="f848b-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ServiceChannel.</span></span>

<span data-ttu-id="f848b-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ServiceChannel, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f848b-142">To configure and test Azure AD single sign-on with ServiceChannel, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f848b-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f848b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f848b-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f848b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f848b-145">**[Vytvoření zkušebního uživatele ServiceChannel](#creating-a-servicechannel-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f848b-145">**[Creating a ServiceChannel test user](#creating-a-servicechannel-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="f848b-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f848b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f848b-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f848b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f848b-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f848b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f848b-149">V této části můžete povolit Azure AD jednotné přihlašování v portálu pro správu Azure a nakonfigurovat jednotné přihlašování v aplikaci ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="f848b-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your ServiceChannel application.</span></span>

<span data-ttu-id="f848b-150">**Ke konfiguraci Azure AD jednotné přihlašování s ServiceChannel, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f848b-150">**To configure Azure AD single sign-on with ServiceChannel, perform the following steps:**</span></span>

1. <span data-ttu-id="f848b-151">Na portálu Azure Management portal na **ServiceChannel** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f848b-151">In the Azure Management portal, on the **ServiceChannel** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f848b-153">Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování na.</span><span class="sxs-lookup"><span data-stu-id="f848b-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. <span data-ttu-id="f848b-155">Na **ServiceChannel domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f848b-155">On the **ServiceChannel Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    <span data-ttu-id="f848b-157">a.</span><span class="sxs-lookup"><span data-stu-id="f848b-157">a.</span></span> <span data-ttu-id="f848b-158">V **identifikátor** textovému poli, zadejte hodnotu jako:`http://adfs.<domain>.com/adfs/service/trust`</span><span class="sxs-lookup"><span data-stu-id="f848b-158">In the **Identifier** textbox, type the value as: `http://adfs.<domain>.com/adfs/service/trust`</span></span>

    <span data-ttu-id="f848b-159">b.</span><span class="sxs-lookup"><span data-stu-id="f848b-159">b.</span></span> <span data-ttu-id="f848b-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<customer domain>.servicechannel.com/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="f848b-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<customer domain>.servicechannel.com/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f848b-161">Upozorňujeme, že tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f848b-161">Please note that these are not the real values.</span></span> <span data-ttu-id="f848b-162">Budete muset aktualizovat tyto hodnoty se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f848b-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="f848b-163">Zde, doporučujeme vám použít jedinečnou hodnotu řetězce v identifikátoru.</span><span class="sxs-lookup"><span data-stu-id="f848b-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="f848b-164">Obraťte se na [tým podpory ServiceChannel](https://servicechannel.zendesk.com/hc/en-us) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f848b-164">Contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us) to get these values.</span></span>

4. <span data-ttu-id="f848b-165">Aplikace ServiceChannel očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="f848b-165">Your ServiceChannel application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="f848b-166">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="f848b-166">The following screenshot shows an example for this.</span></span> <span data-ttu-id="f848b-167">**NameIdentifier (identifikátor uživatele)** je povinný jenom deklarace identity a výchozí hodnota je **user.userprincipalname** ale ServiceChannel očekává to nejde mapovat s **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="f848b-167">**NameIdentifier(User Identifier)** is the only mandatory claim and the default value is **user.userprincipalname** but ServiceChannel expects this to be mapped with **user.mail**.</span></span> <span data-ttu-id="f848b-168">Pokud máte v úmyslu povolit zřizování uživatelů jenom v době, by měl jak je uvedeno níže přidejte následující deklarace identity.</span><span class="sxs-lookup"><span data-stu-id="f848b-168">If you are planning to enable Just In Time user provisioning, then you should add the following claims as shown below.</span></span> <span data-ttu-id="f848b-169">**Role** deklarace identity musí být namapovaný na **user.assignedroles** obsahující roli uživatele.</span><span class="sxs-lookup"><span data-stu-id="f848b-169">**Role** claim needs to be mapped to **user.assignedroles** which contains the role of the user.</span></span>  

    <span data-ttu-id="f848b-170">Najdete Průvodce ServiceChannel [sem](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) další pokyny na deklaracích identity.</span><span class="sxs-lookup"><span data-stu-id="f848b-170">You can refer ServiceChannel guide [here](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) for more guidance on claims.</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > <span data-ttu-id="f848b-172">Kliknutím na [sem](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) vědět, jak nakonfigurovat **Role** ve službě Azure AD</span><span class="sxs-lookup"><span data-stu-id="f848b-172">Please click [here](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) to know how to configure **Role** in Azure AD</span></span>

5. <span data-ttu-id="f848b-173">V **uživatelské atributy** klikněte na tlačítko **zobrazit a upravit všechny ostatní atributy uživatele** a nastavit atributy.</span><span class="sxs-lookup"><span data-stu-id="f848b-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span>

    | <span data-ttu-id="f848b-174">Název atributu</span><span class="sxs-lookup"><span data-stu-id="f848b-174">Attribute Name</span></span> | <span data-ttu-id="f848b-175">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="f848b-175">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="f848b-176">Role</span><span class="sxs-lookup"><span data-stu-id="f848b-176">Role</span></span>| <span data-ttu-id="f848b-177">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="f848b-177">user.assignedroles</span></span> |

    <span data-ttu-id="f848b-178">a.</span><span class="sxs-lookup"><span data-stu-id="f848b-178">a.</span></span> <span data-ttu-id="f848b-179">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    <span data-ttu-id="f848b-182">b.</span><span class="sxs-lookup"><span data-stu-id="f848b-182">b.</span></span> <span data-ttu-id="f848b-183">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="f848b-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="f848b-184">c.</span><span class="sxs-lookup"><span data-stu-id="f848b-184">c.</span></span> <span data-ttu-id="f848b-185">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="f848b-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="f848b-186">d.</span><span class="sxs-lookup"><span data-stu-id="f848b-186">d.</span></span> <span data-ttu-id="f848b-187">Klikněte na tlačítko **Ok**</span><span class="sxs-lookup"><span data-stu-id="f848b-187">Click **Ok**</span></span>
    
6. <span data-ttu-id="f848b-188">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="f848b-188">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. <span data-ttu-id="f848b-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="f848b-190">Click **Save**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f848b-192">Na **ServiceChannel konfigurace** klikněte na tlačítko **konfigurace ServiceChannel** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-192">On the **ServiceChannel Configuration** section, click **Configure ServiceChannel** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f848b-193">Upozorňujeme **SAML Enitity ID** z **Stručná referenční příručka** části.</span><span class="sxs-lookup"><span data-stu-id="f848b-193">Please note the **SAML Enitity ID** from the **Quick Reference** section.</span></span>

9. <span data-ttu-id="f848b-194">Konfigurace jednotného přihlašování na **ServiceChannel** straně, budete muset odeslat stažené **certifikátu (Base64)** a **SAML Entity ID** k [ServiceChannel tým podpory](https://servicechannel.zendesk.com/hc/en-us).</span><span class="sxs-lookup"><span data-stu-id="f848b-194">To configure single sign-on on **ServiceChannel** side, you need to send the downloaded **certificate (Base64)** and **SAML Entity ID** to [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us).</span></span> <span data-ttu-id="f848b-195">Toto se bude nastavení aby bylo možné používat jednotné přihlašování SAML připojení správně nastavena na obou stranách.</span><span class="sxs-lookup"><span data-stu-id="f848b-195">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f848b-196">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f848b-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="f848b-197">Cílem této části je vytvoření zkušebního uživatele na portálu správy Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f848b-197">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f848b-199">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f848b-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f848b-200">V **portálu pro správu Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f848b-200">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f848b-202">Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f848b-202">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f848b-204">V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-204">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f848b-206">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f848b-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f848b-208">a.</span><span class="sxs-lookup"><span data-stu-id="f848b-208">a.</span></span> <span data-ttu-id="f848b-209">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f848b-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f848b-210">b.</span><span class="sxs-lookup"><span data-stu-id="f848b-210">b.</span></span> <span data-ttu-id="f848b-211">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f848b-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f848b-212">c.</span><span class="sxs-lookup"><span data-stu-id="f848b-212">c.</span></span> <span data-ttu-id="f848b-213">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f848b-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f848b-214">d.</span><span class="sxs-lookup"><span data-stu-id="f848b-214">d.</span></span> <span data-ttu-id="f848b-215">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f848b-215">Click **Create**.</span></span> 

### <a name="creating-a-servicechannel-test-user"></a><span data-ttu-id="f848b-216">Vytvoření zkušebního uživatele ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="f848b-216">Creating a ServiceChannel test user</span></span>

<span data-ttu-id="f848b-217">Aplikace podporuje pouze v době zřizování uživatelů a po ověření uživatele automaticky se vytvoří v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f848b-217">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="f848b-218">Pro úplné uživatelské zřizování, obraťte se na [ServiceChannel tým podpory](https://servicechannel.zendesk.com/hc/en-us)</span><span class="sxs-lookup"><span data-stu-id="f848b-218">For full user provisioning, please contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us)</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f848b-219">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f848b-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f848b-220">V této části povolíte Britta Simon používat tak, že udělíte přístup k ServiceChannel Azure jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f848b-220">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to ServiceChannel.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f848b-222">**Pokud chcete přiřadit Britta Simon ServiceChannel, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f848b-222">**To assign Britta Simon to ServiceChannel, perform the following steps:**</span></span>

1. <span data-ttu-id="f848b-223">V portálu pro správu Azure, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f848b-223">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f848b-225">V seznamu aplikací vyberte **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="f848b-225">In the applications list, select **ServiceChannel**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. <span data-ttu-id="f848b-227">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f848b-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f848b-229">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f848b-229">Click **Add** button.</span></span> <span data-ttu-id="f848b-230">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f848b-232">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f848b-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f848b-233">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f848b-234">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f848b-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f848b-235">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f848b-235">Testing single sign-on</span></span>

<span data-ttu-id="f848b-236">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f848b-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f848b-237">Když kliknete na dlaždici ServiceChannel na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="f848b-237">When you click the ServiceChannel tile in the Access Panel, you should get automatically signed-on to your ServiceChannel application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f848b-238">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f848b-238">Additional resources</span></span>

* [<span data-ttu-id="f848b-239">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f848b-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f848b-240">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f848b-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png