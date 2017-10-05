---
title: 'Kurz: Azure Active Directory integrace s ADP eTime | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a ADP eTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 31fed307a32e629d00aab7cc9d5167ee16d83936
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a><span data-ttu-id="2a1ff-103">Kurz: Azure Active Directory integrace s ADP eTime</span><span class="sxs-lookup"><span data-stu-id="2a1ff-103">Tutorial: Azure Active Directory integration with ADP eTime</span></span>

<span data-ttu-id="2a1ff-104">V tomto kurzu zjistěte, jak integrovat ADP eTime s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2a1ff-104">In this tutorial, you learn how to integrate ADP eTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2a1ff-105">Integrace ADP eTime s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-105">Integrating ADP eTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2a1ff-106">Můžete řídit ve službě Azure AD, který má přístup k ADP eTime</span><span class="sxs-lookup"><span data-stu-id="2a1ff-106">You can control in Azure AD who has access to ADP eTime</span></span>
- <span data-ttu-id="2a1ff-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k ADP eTime (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="2a1ff-107">You can enable your users to automatically get signed-on to ADP eTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2a1ff-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="2a1ff-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2a1ff-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2a1ff-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2a1ff-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2a1ff-110">Prerequisites</span></span>

<span data-ttu-id="2a1ff-111">Konfigurace integrace Azure AD s ADP eTime, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-111">To configure Azure AD integration with ADP eTime, you need the following items:</span></span>

- <span data-ttu-id="2a1ff-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="2a1ff-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2a1ff-113">ADP eTime jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="2a1ff-113">An ADP eTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2a1ff-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2a1ff-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2a1ff-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2a1ff-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2a1ff-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2a1ff-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="2a1ff-118">Scenario description</span></span>
<span data-ttu-id="2a1ff-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2a1ff-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2a1ff-121">Přidání ADP eTime z Galerie</span><span class="sxs-lookup"><span data-stu-id="2a1ff-121">Adding ADP eTime from the gallery</span></span>
2. <span data-ttu-id="2a1ff-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2a1ff-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-etime-from-the-gallery"></a><span data-ttu-id="2a1ff-123">Přidání ADP eTime z Galerie</span><span class="sxs-lookup"><span data-stu-id="2a1ff-123">Adding ADP eTime from the gallery</span></span>
<span data-ttu-id="2a1ff-124">Při konfiguraci integrace ADP eTime do služby Azure AD potřebujete přidat ADP eTime z Galerie si na seznam spravovaných aplikací SaaS.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-124">To configure the integration of ADP eTime into Azure AD, you need to add ADP eTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2a1ff-125">**Pokud chcete přidat ADP eTime z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2a1ff-125">**To add ADP eTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2a1ff-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2a1ff-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2a1ff-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="2a1ff-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="2a1ff-133">Do vyhledávacího pole zadejte **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-133">In the search box, type **ADP eTime**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. <span data-ttu-id="2a1ff-135">Na panelu výsledků vyberte **ADP eTime**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-135">In the results panel, select **ADP eTime**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2a1ff-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="2a1ff-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2a1ff-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s ADP eTime podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="2a1ff-138">In this section, you configure and test Azure AD single sign-on with ADP eTime based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2a1ff-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v ADP eTime je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ADP eTime is to a user in Azure AD.</span></span> <span data-ttu-id="2a1ff-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v ADP eTime musí navázat.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-140">In other words, a link relationship between an Azure AD user and the related user in ADP eTime needs to be established.</span></span>

<span data-ttu-id="2a1ff-141">Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ADP eTime.</span></span>

<span data-ttu-id="2a1ff-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s ADP eTime, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-142">To configure and test Azure AD single sign-on with ADP eTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2a1ff-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2a1ff-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2a1ff-145">**[Vytváření testovacího uživatele ADP eTime](#creating-an-adp-etime-test-user)**  – Pokud chcete mít protějšek Britta Simon v ADP eTime propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-145">**[Creating an ADP eTime test user](#creating-an-adp-etime-test-user)** - to have a counterpart of Britta Simon in ADP eTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2a1ff-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2a1ff-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2a1ff-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2a1ff-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2a1ff-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci eTime ADP.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ADP eTime application.</span></span>

<span data-ttu-id="2a1ff-150">**Ke konfiguraci Azure AD jednotné přihlašování s ADP eTime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2a1ff-150">**To configure Azure AD single sign-on with ADP eTime, perform the following steps:**</span></span>

1. <span data-ttu-id="2a1ff-151">Na portálu Azure na **ADP eTime** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-151">In the Azure portal, on the **ADP eTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="2a1ff-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. <span data-ttu-id="2a1ff-155">Na **ADP eTime domény a adresy URL** část, proveďte následující krok:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-155">On the **ADP eTime Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    <span data-ttu-id="2a1ff-157">a.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-157">a.</span></span> <span data-ttu-id="2a1ff-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<servername>.adp.com`</span><span class="sxs-lookup"><span data-stu-id="2a1ff-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<servername>.adp.com`</span></span>

    <span data-ttu-id="2a1ff-159">b.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-159">b.</span></span> <span data-ttu-id="2a1ff-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="2a1ff-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span></span> 
 
    > [!NOTE] 
    > <span data-ttu-id="2a1ff-161">Tyto hodnoty nejsou reálné.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-161">These values are not the real.</span></span> <span data-ttu-id="2a1ff-162">Tyto hodnoty aktualizujte skutečná adresa URL odpovědi a identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-162">Update these values with the actual Reply URL and Identifier.</span></span> <span data-ttu-id="2a1ff-163">Obraťte se na [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-163">Contact [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) to get these values.</span></span>

4. <span data-ttu-id="2a1ff-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. <span data-ttu-id="2a1ff-166">Aplikace eTime ADP očekává SAML kontrolní výrazy ve specifickém formátu, který vyžaduje, můžete přidat mapování vlastních atributů do vaší konfigurace atributy tokenu SAML.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-166">The ADP eTime application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="2a1ff-167">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-167">The following screenshot shows an example for this.</span></span> <span data-ttu-id="2a1ff-168">Název deklarací bude vždy **"PersonImmutableID"** a hodnoty, které jsme jsou namapovány ExtensionAttribute2, který obsahuje EmployeeID uživatele.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-168">The claim name will always be **"PersonImmutableID"** and the value of which we have mapped to ExtensionAttribute2 which contains the EmployeeID of the user.</span></span> 

    <span data-ttu-id="2a1ff-169">Zde namapování uživatele z Azure AD na ADP eTime bude provedeno v EmployeeID, ale to můžete namapovat na jinou hodnotu také podle nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-169">Here the user mapping from Azure AD to ADP eTime will be done on the EmployeeID but you can map this to a different value also based on your application settings.</span></span> <span data-ttu-id="2a1ff-170">Proto prosím práci s [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) nejprve k použijte správný identifikátor uživatele a mapování danou hodnotu s **"PersonImmutableID"** deklarací identity.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-170">So please work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) first to use the correct identifier of a user and map that value with the **"PersonImmutableID"** claim.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. <span data-ttu-id="2a1ff-172">V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-172">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="2a1ff-173">Název atributu</span><span class="sxs-lookup"><span data-stu-id="2a1ff-173">Attribute Name</span></span> | <span data-ttu-id="2a1ff-174">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="2a1ff-174">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="2a1ff-175">PersonImmutableID</span><span class="sxs-lookup"><span data-stu-id="2a1ff-175">PersonImmutableID</span></span> | <span data-ttu-id="2a1ff-176">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="2a1ff-176">user.extensionattribute2</span></span> |
    
    <span data-ttu-id="2a1ff-177">a.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-177">a.</span></span> <span data-ttu-id="2a1ff-178">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-178">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="2a1ff-181">b.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-181">b.</span></span> <span data-ttu-id="2a1ff-182">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-182">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="2a1ff-183">c.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-183">c.</span></span> <span data-ttu-id="2a1ff-184">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-184">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="2a1ff-185">d.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-185">d.</span></span> <span data-ttu-id="2a1ff-186">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-186">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2a1ff-187">Před konfigurací kontrolního výrazu SAML, budete muset kontaktovat vaše [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) a požadovat hodnotu atributu jedinečný identifikátor pro vašeho klienta.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-187">Before you can configure the SAML assertion, you need to contact your [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="2a1ff-188">Je nutné tuto hodnotu ke konfiguraci vlastních deklarací identity pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-188">You need this value to configure the custom claim for your application.</span></span> 

7. <span data-ttu-id="2a1ff-189">Na **ADP eTime konfigurace** klikněte na tlačítko **konfigurace ADP eTime** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-189">On the **ADP eTime Configuration** section, click **Configure ADP eTime** to open **Configure sign-on** window.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. <span data-ttu-id="2a1ff-191">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-191">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="2a1ff-193">Konfigurace jednotného přihlašování na **ADP eTime** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a1ff-193">To configure single sign-on on **ADP eTime** side, you need to send the downloaded **Metadata XML** to [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx).</span></span> 

> [!TIP]
> <span data-ttu-id="2a1ff-194">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="2a1ff-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2a1ff-195">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2a1ff-196">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2a1ff-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2a1ff-197">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2a1ff-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="2a1ff-198">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="2a1ff-200">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2a1ff-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2a1ff-201">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2a1ff-203">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2a1ff-205">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2a1ff-207">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2a1ff-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2a1ff-209">a.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-209">a.</span></span> <span data-ttu-id="2a1ff-210">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2a1ff-211">b.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-211">b.</span></span> <span data-ttu-id="2a1ff-212">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2a1ff-213">c.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-213">c.</span></span> <span data-ttu-id="2a1ff-214">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2a1ff-215">d.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-215">d.</span></span> <span data-ttu-id="2a1ff-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-216">Click **Create**.</span></span>
 
### <a name="creating-an-adp-etime-test-user"></a><span data-ttu-id="2a1ff-217">Vytvoření ADP eTime testovacího uživatele</span><span class="sxs-lookup"><span data-stu-id="2a1ff-217">Creating an ADP eTime test user</span></span>

<span data-ttu-id="2a1ff-218">Cílem této části je vytvoření uživatele v ADP eTime nazývá Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-218">The objective of this section is to create a user called Britta Simon in ADP eTime.</span></span> <span data-ttu-id="2a1ff-219">Práce s [tým podpory eTime ADP](https://www.adp.com/contact-us/overview.aspx) přidat uživatele do účtu eTime ADP.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-219">Work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) to add the users in the ADP eTime account.</span></span> 
   
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2a1ff-220">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="2a1ff-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2a1ff-221">V této části povolíte Britta Simon chcete použít Azure jednotného přihlašování k udělení přístupu k ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ADP eTime.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="2a1ff-223">**Pokud chcete přiřadit Britta Simon ADP eTime, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="2a1ff-223">**To assign Britta Simon to ADP eTime, perform the following steps:**</span></span>

1. <span data-ttu-id="2a1ff-224">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="2a1ff-226">V seznamu aplikací vyberte **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-226">In the applications list, select **ADP eTime**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. <span data-ttu-id="2a1ff-228">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="2a1ff-230">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-230">Click **Add** button.</span></span> <span data-ttu-id="2a1ff-231">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="2a1ff-233">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2a1ff-234">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2a1ff-235">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2a1ff-236">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="2a1ff-236">Testing single sign-on</span></span>

<span data-ttu-id="2a1ff-237">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2a1ff-238">Když kliknete na dlaždici eTime ADP na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci eTime ADP.</span><span class="sxs-lookup"><span data-stu-id="2a1ff-238">When you click the ADP eTime tile in the Access Panel, you should get automatically signed-on to your ADP eTime application.</span></span>
<span data-ttu-id="2a1ff-239">Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2a1ff-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2a1ff-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="2a1ff-240">Additional resources</span></span>

* [<span data-ttu-id="2a1ff-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2a1ff-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2a1ff-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2a1ff-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

