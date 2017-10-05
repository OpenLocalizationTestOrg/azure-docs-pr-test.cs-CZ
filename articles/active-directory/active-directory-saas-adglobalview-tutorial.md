---
title: 'Kurz: Azure Active Directory integrace s ADP Globalview | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ADP Globalview."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffb6464f-714d-41a9-869a-2b7e5ae9f125
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: e9a5e65c484dfb98d1a7bc63d55f6ef92039554b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-globalview"></a><span data-ttu-id="9ea2f-103">Kurz: Azure Active Directory integrace s ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="9ea2f-103">Tutorial: Azure Active Directory integration with ADP Globalview</span></span>

<span data-ttu-id="9ea2f-104">V tomto kurzu zjistěte, jak integrovat ADP Globalview s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ea2f-104">In this tutorial, you learn how to integrate ADP Globalview with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ea2f-105">Integrace ADP Globalview s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-105">Integrating ADP Globalview with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9ea2f-106">Můžete řídit ve službě Azure AD, který má přístup k ADP Globalview</span><span class="sxs-lookup"><span data-stu-id="9ea2f-106">You can control in Azure AD who has access to ADP Globalview</span></span>
- <span data-ttu-id="9ea2f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ADP Globalview (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ea2f-107">You can enable your users to automatically get signed-on to ADP Globalview (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9ea2f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="9ea2f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9ea2f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ea2f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ea2f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="9ea2f-110">Prerequisites</span></span>

<span data-ttu-id="9ea2f-111">Konfigurace integrace Azure AD s ADP Globalview, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-111">To configure Azure AD integration with ADP Globalview, you need the following items:</span></span>

- <span data-ttu-id="9ea2f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ea2f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ea2f-113">ADP Globalview jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="9ea2f-113">An ADP Globalview single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ea2f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ea2f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ea2f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ea2f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ea2f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ea2f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="9ea2f-118">Scenario description</span></span>
<span data-ttu-id="9ea2f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ea2f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ea2f-121">Přidání ADP Globalview z Galerie</span><span class="sxs-lookup"><span data-stu-id="9ea2f-121">Adding ADP Globalview from the gallery</span></span>
2. <span data-ttu-id="9ea2f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ea2f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-globalview-from-the-gallery"></a><span data-ttu-id="9ea2f-123">Přidání ADP Globalview z Galerie</span><span class="sxs-lookup"><span data-stu-id="9ea2f-123">Adding ADP Globalview from the gallery</span></span>
<span data-ttu-id="9ea2f-124">Při konfiguraci integrace ADP Globalview do služby Azure AD potřebujete přidat ADP Globalview z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-124">To configure the integration of ADP Globalview into Azure AD, you need to add ADP Globalview from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9ea2f-125">**Pokud chcete přidat ADP Globalview z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ea2f-125">**To add ADP Globalview from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9ea2f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9ea2f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9ea2f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="9ea2f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="9ea2f-133">Do vyhledávacího pole zadejte **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-133">In the search box, type **ADP Globalview**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_search.png)

5. <span data-ttu-id="9ea2f-135">Na panelu výsledků vyberte **ADP Globalview**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-135">In the results panel, select **ADP Globalview**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9ea2f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ea2f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9ea2f-138">V této části nakonfigurujete a testu Azure AD jednotné přihlašování s ADP Globalview podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="9ea2f-138">In this section, you configure and test Azure AD single sign-on with ADP Globalview based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9ea2f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ADP Globalview je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ADP Globalview is to a user in Azure AD.</span></span> <span data-ttu-id="9ea2f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ADP Globalview musí navázat.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-140">In other words, a link relationship between an Azure AD user and the related user in ADP Globalview needs to be established.</span></span>

<span data-ttu-id="9ea2f-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ADP Globalview.</span></span>

<span data-ttu-id="9ea2f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ADP Globalview, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-142">To configure and test Azure AD single sign-on with ADP Globalview, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9ea2f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9ea2f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ea2f-145">**[Vytváření testovacího uživatele ADP Globalview](#creating-an-adp-globalview-test-user)**  – Pokud chcete mít protějšek Britta Simon v ADP Globalview propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-145">**[Creating an ADP Globalview test user](#creating-an-adp-globalview-test-user)** - to have a counterpart of Britta Simon in ADP Globalview that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ea2f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ea2f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9ea2f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ea2f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9ea2f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ADP Globalview application.</span></span>

<span data-ttu-id="9ea2f-150">**Ke konfiguraci Azure AD jednotné přihlašování s ADP Globalview, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ea2f-150">**To configure Azure AD single sign-on with ADP Globalview, perform the following steps:**</span></span>

1. <span data-ttu-id="9ea2f-151">Na portálu Azure na **ADP Globalview** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-151">In the Azure portal, on the **ADP Globalview** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="9ea2f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_samlbase.png)

3. <span data-ttu-id="9ea2f-155">Na **ADP Globalview domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-155">On the **ADP Globalview Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_url.png)

     <span data-ttu-id="9ea2f-157">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce: `https://<subdomain>.globalview.adp.com/federate` nebo`https://<subdomain>.globalview.adp.com/federate2`</span><span class="sxs-lookup"><span data-stu-id="9ea2f-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.globalview.adp.com/federate` or `https://<subdomain>.globalview.adp.com/federate2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ea2f-158">Hodnota není skutečné.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-158">The value is not real.</span></span> <span data-ttu-id="9ea2f-159">Aktualizujte hodnotu skutečným identifikátorem.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-159">Update the value with the actual Identifier.</span></span> <span data-ttu-id="9ea2f-160">Obraťte se na [ADP Globalview podporu](https://www.adp.com/contact-us/overview.aspx) k získání hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-160">Contact [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) to get the value.</span></span>
 
4. <span data-ttu-id="9ea2f-161">Na **SAML podpisový certifikát** klikněte na tlačítko **certifikátu (Base64)** a potom uložte soubor certifikátu v počítači.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_certificate.png) 

5. <span data-ttu-id="9ea2f-163">Aplikace ADP GlobalView očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-163">The ADP GlobalView application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> 

6. <span data-ttu-id="9ea2f-164">Následující snímek obrazovky ukazuje příklad pro ni.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-164">The following screenshot shows an example for it.</span></span> <span data-ttu-id="9ea2f-165">Názvy deklarace identity se vždycky být **"PersonImmutableID"** a hodnoty, které jsme jsou namapovány ExtensionAttribute2, který obsahuje EmployeeID uživatele.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-165">The claim names always be **"PersonImmutableID"** and the value of which we have mapped to ExtensionAttribute2, which contains the EmployeeID of the user.</span></span> <span data-ttu-id="9ea2f-166">Zde namapování uživatele z Azure AD na ADP GlobalView probíhá na EmployeeID ale můžete ji namapovat na jinou hodnotu také podle nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-166">Here the user mapping from Azure AD to ADP GlobalView is done on the EmployeeID but you can map it to a different value also based on your application settings.</span></span> <span data-ttu-id="9ea2f-167">Můžete spolupracovat s týmem ADP GlobalView nejprve k použijte správný identifikátor uživatele a mapování danou hodnotu s **"PersonImmutableID"** deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-167">You can work with the ADP GlobalView team first to use the correct identifier of a user and map that value with the **"PersonImmutableID"** claim.</span></span> <span data-ttu-id="9ea2f-168">Můžete také mapovat deklarace identity e-mailu a ID uživatele, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-168">You can also map the Email and UserID claim as shown in the picture.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_attribute.png)

7. <span data-ttu-id="9ea2f-170">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="9ea2f-171">Název atributu</span><span class="sxs-lookup"><span data-stu-id="9ea2f-171">Attribute Name</span></span> | <span data-ttu-id="9ea2f-172">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="9ea2f-172">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="9ea2f-173">personalimmutableid</span><span class="sxs-lookup"><span data-stu-id="9ea2f-173">personalimmutableid</span></span> | <span data-ttu-id="9ea2f-174">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="9ea2f-174">user.extensionattribute2</span></span> |
    | <span data-ttu-id="9ea2f-175">E-mailu</span><span class="sxs-lookup"><span data-stu-id="9ea2f-175">email</span></span>               | <span data-ttu-id="9ea2f-176">User.Mail</span><span class="sxs-lookup"><span data-stu-id="9ea2f-176">user.mail</span></span> |
    | <span data-ttu-id="9ea2f-177">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="9ea2f-177">userid</span></span>              | <span data-ttu-id="9ea2f-178">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="9ea2f-178">user.userprincipalname</span></span>|
    
    <span data-ttu-id="9ea2f-179">a.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-179">a.</span></span> <span data-ttu-id="9ea2f-180">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-180">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="9ea2f-183">b.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-183">b.</span></span> <span data-ttu-id="9ea2f-184">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="9ea2f-185">c.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-185">c.</span></span> <span data-ttu-id="9ea2f-186">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-186">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="9ea2f-187">d.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-187">d.</span></span> <span data-ttu-id="9ea2f-188">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-188">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ea2f-189">Před konfigurací kontrolního výrazu SAML, budete muset kontaktovat vaše [ADP Globalview podporu](https://www.adp.com/contact-us/overview.aspx) a požadovat hodnotu atributu jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-189">Before you can configure the SAML assertion, you need to contact your [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="9ea2f-190">Je nutné tuto hodnotu ke konfiguraci vlastních deklarací identity pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-190">You need this value to configure the custom claim for your application.</span></span> 

8. <span data-ttu-id="9ea2f-191">Na **ADP Globalview konfigurace** klikněte na tlačítko **konfigurace ADP Globalview** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-191">On the **ADP Globalview Configuration** section, click **Configure ADP Globalview** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9ea2f-192">Kopírování **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="9ea2f-192">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_configure.png) 

9. <span data-ttu-id="9ea2f-194">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-194">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="9ea2f-196">Konfigurace jednotného přihlašování na **ADP Globalview** straně, budete muset odeslat stažené **certifikátu (Base64)**, **Sign-Out adresu URL, SAML Entity ID a SAML jeden přihlašování adresa URL služby** k [ADP Globalview podporu](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ea2f-196">To configure single sign-on on **ADP Globalview** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx).</span></span>

> [!TIP]
> <span data-ttu-id="9ea2f-197">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="9ea2f-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9ea2f-198">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9ea2f-199">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ea2f-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9ea2f-200">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ea2f-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="9ea2f-201">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="9ea2f-203">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ea2f-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9ea2f-204">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_01.png) 

2.  <span data-ttu-id="9ea2f-206">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9ea2f-208">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9ea2f-210">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9ea2f-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adglobalview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9ea2f-212">a.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-212">a.</span></span> <span data-ttu-id="9ea2f-213">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ea2f-214">b.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-214">b.</span></span> <span data-ttu-id="9ea2f-215">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9ea2f-216">c.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-216">c.</span></span> <span data-ttu-id="9ea2f-217">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9ea2f-218">d.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-218">d.</span></span> <span data-ttu-id="9ea2f-219">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-219">Click **Create**.</span></span>
 
### <a name="creating-an-adp-globalview-test-user"></a><span data-ttu-id="9ea2f-220">Vytvoření ADP Globalview testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="9ea2f-220">Creating an ADP Globalview test user</span></span>

<span data-ttu-id="9ea2f-221">Cílem této části je vytvoření uživatele v ADP GlobalView nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-221">The objective of this section is to create a user called Britta Simon in ADP GlobalView.</span></span> <span data-ttu-id="9ea2f-222">Práce s [ADP Globalview podporu](https://www.adp.com/contact-us/overview.aspx) přidat uživatele do ADP GlobalView účtu.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-222">Work with [ADP Globalview support](https://www.adp.com/contact-us/overview.aspx) to add the users in the ADP GlobalView account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9ea2f-223">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="9ea2f-223">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9ea2f-224">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu ADP Globalview.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-224">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ADP Globalview.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="9ea2f-226">**Pokud chcete přiřadit Britta Simon ADP Globalview, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="9ea2f-226">**To assign Britta Simon to ADP Globalview, perform the following steps:**</span></span>

1. <span data-ttu-id="9ea2f-227">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-227">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="9ea2f-229">V seznamu aplikací vyberte **ADP Globalview**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-229">In the applications list, select **ADP Globalview**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adglobalview-tutorial/tutorial_adpglobalview_app.png) 

3. <span data-ttu-id="9ea2f-231">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-231">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="9ea2f-233">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-233">Click **Add** button.</span></span> <span data-ttu-id="9ea2f-234">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="9ea2f-236">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-236">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9ea2f-237">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ea2f-238">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9ea2f-239">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="9ea2f-239">Testing single sign-on</span></span>

<span data-ttu-id="9ea2f-240">Cílem této části je testování konfigurace Azure AD jednotného přihlašování k použití na přístupovém panelu.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-240">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="9ea2f-241">Když kliknete na dlaždici ADP GlobalView na přístupovém panelu, můžete by měl získat automaticky přihlášení k aplikaci ADP GlobalView.</span><span class="sxs-lookup"><span data-stu-id="9ea2f-241">When you click the ADP GlobalView tile in the Access Panel, you should get automatically signed-on to your ADP GlobalView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ea2f-242">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="9ea2f-242">Additional resources</span></span>

* [<span data-ttu-id="9ea2f-243">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9ea2f-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ea2f-244">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9ea2f-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adglobalview-tutorial/tutorial_general_203.png

