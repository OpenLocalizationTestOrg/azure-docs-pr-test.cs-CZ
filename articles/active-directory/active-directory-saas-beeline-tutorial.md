---
title: 'Kurz: Azure Active Directory integrace s BeeLine | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BeeLine."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 93acbd90bbe5f0a40bf3f56edb766a0fdd30f68f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a><span data-ttu-id="a961f-103">Kurz: Azure Active Directory integrace s BeeLine</span><span class="sxs-lookup"><span data-stu-id="a961f-103">Tutorial: Azure Active Directory integration with BeeLine</span></span>

<span data-ttu-id="a961f-104">V tomto kurzu zjistěte, jak integrovat BeeLine s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a961f-104">In this tutorial, you learn how to integrate BeeLine with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a961f-105">Integrace BeeLine s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="a961f-105">Integrating BeeLine with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a961f-106">Můžete řídit ve službě Azure AD, který má přístup k BeeLine</span><span class="sxs-lookup"><span data-stu-id="a961f-106">You can control in Azure AD who has access to BeeLine</span></span>
- <span data-ttu-id="a961f-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BeeLine (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="a961f-107">You can enable your users to automatically get signed-on to BeeLine (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a961f-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a961f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a961f-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a961f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a961f-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a961f-110">Prerequisites</span></span>

<span data-ttu-id="a961f-111">Konfigurace integrace Azure AD s BeeLine, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="a961f-111">To configure Azure AD integration with BeeLine, you need the following items:</span></span>

- <span data-ttu-id="a961f-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="a961f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a961f-113">BeeLine jednotné přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="a961f-113">A BeeLine single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a961f-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="a961f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a961f-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="a961f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a961f-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="a961f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a961f-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a961f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a961f-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="a961f-118">Scenario description</span></span>
<span data-ttu-id="a961f-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="a961f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a961f-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="a961f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a961f-121">Přidání BeeLine z Galerie</span><span class="sxs-lookup"><span data-stu-id="a961f-121">Adding BeeLine from the gallery</span></span>
2. <span data-ttu-id="a961f-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a961f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-beeline-from-the-gallery"></a><span data-ttu-id="a961f-123">Přidání BeeLine z Galerie</span><span class="sxs-lookup"><span data-stu-id="a961f-123">Adding BeeLine from the gallery</span></span>
<span data-ttu-id="a961f-124">Při konfiguraci integrace BeeLine do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BeeLine z galerie.</span><span class="sxs-lookup"><span data-stu-id="a961f-124">To configure the integration of BeeLine into Azure AD, you need to add BeeLine from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a961f-125">**Pokud chcete přidat BeeLine z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a961f-125">**To add BeeLine from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a961f-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a961f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a961f-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="a961f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a961f-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a961f-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="a961f-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a961f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="a961f-133">Do vyhledávacího pole zadejte **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="a961f-133">In the search box, type **BeeLine**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_search.png)

5. <span data-ttu-id="a961f-135">Na panelu výsledků vyberte **BeeLine**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a961f-135">In the results panel, select **BeeLine**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a961f-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="a961f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a961f-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BeeLine podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="a961f-138">In this section, you configure and test Azure AD single sign-on with BeeLine based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a961f-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BeeLine je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a961f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BeeLine is to a user in Azure AD.</span></span> <span data-ttu-id="a961f-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BeeLine musí navázat.</span><span class="sxs-lookup"><span data-stu-id="a961f-140">In other words, a link relationship between an Azure AD user and the related user in BeeLine needs to be established.</span></span>

<span data-ttu-id="a961f-141">V BeeLine, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="a961f-141">In BeeLine, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a961f-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BeeLine, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="a961f-142">To configure and test Azure AD single sign-on with BeeLine, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a961f-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="a961f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a961f-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a961f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a961f-145">**[Vytvoření zkušebního uživatele BeeLine](#creating-a-beeline-test-user)**  – Pokud chcete mít protějšek Britta Simon v BeeLine propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="a961f-145">**[Creating a BeeLine test user](#creating-a-beeline-test-user)** - to have a counterpart of Britta Simon in BeeLine that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a961f-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a961f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a961f-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="a961f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a961f-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a961f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a961f-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BeeLine.</span><span class="sxs-lookup"><span data-stu-id="a961f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BeeLine application.</span></span>

<span data-ttu-id="a961f-150">**Ke konfiguraci Azure AD jednotné přihlašování s BeeLine, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a961f-150">**To configure Azure AD single sign-on with BeeLine, perform the following steps:**</span></span>

1. <span data-ttu-id="a961f-151">Na portálu Azure na **BeeLine** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="a961f-151">In the Azure portal, on the **BeeLine** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="a961f-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a961f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_samlbase.png)

3. <span data-ttu-id="a961f-155">Na **BeeLine domény a adresy URL** část, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a961f-155">On the **BeeLine Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_url.png)

    <span data-ttu-id="a961f-157">a.</span><span class="sxs-lookup"><span data-stu-id="a961f-157">a.</span></span> <span data-ttu-id="a961f-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://projects.beeline.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="a961f-158">In the **Identifier** textbox, type a URL using the following pattern: `https://projects.beeline.net/<instancename>`</span></span>

    <span data-ttu-id="a961f-159">b.</span><span class="sxs-lookup"><span data-stu-id="a961f-159">b.</span></span> <span data-ttu-id="a961f-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:</span><span class="sxs-lookup"><span data-stu-id="a961f-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > <span data-ttu-id="a961f-161">Tyto hodnoty nejsou skutečné.</span><span class="sxs-lookup"><span data-stu-id="a961f-161">These values are not real.</span></span> <span data-ttu-id="a961f-162">Tyto hodnoty aktualizujte se skutečným identifikátorem a adresa URL odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a961f-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a961f-163">Obraťte se na [tým podpory BeeLine](https://www.beeline.com/contact-us/) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="a961f-163">Contact [BeeLine support team](https://www.beeline.com/contact-us/) to get these values.</span></span>
 
4. <span data-ttu-id="a961f-164">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="a961f-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_certificate.png) 

5. <span data-ttu-id="a961f-166">Aplikace Beeline očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="a961f-166">Your Beeline application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="a961f-167">Spojte se s [tým podpory BeeLine](https://www.beeline.com/contact-us/) nejprve k identifikaci správné uživatelské identifikátor, který budou zmapována do aplikace.</span><span class="sxs-lookup"><span data-stu-id="a961f-167">Please work with [BeeLine support team](https://www.beeline.com/contact-us/) first to identify the correct user identifier which will be mapped into the application.</span></span> <span data-ttu-id="a961f-168">Také proveďte pokynů od [tým podpory BeeLine](https://www.beeline.com/contact-us/) o atribut, který se má použít pro toto mapování.</span><span class="sxs-lookup"><span data-stu-id="a961f-168">Also please take the guidance from [BeeLine support team](https://www.beeline.com/contact-us/) about the attribute which they want to use for this mapping.</span></span> <span data-ttu-id="a961f-169">Hodnota tohoto atributu z můžete spravovat **uživatelské atributy** aplikace.</span><span class="sxs-lookup"><span data-stu-id="a961f-169">You can manage the value of this attribute from the **User Attributes** tab of the application.</span></span> <span data-ttu-id="a961f-170">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="a961f-170">The following screenshot shows an example for this.</span></span> <span data-ttu-id="a961f-171">Zde jsme jsou namapovány **uživatelský identifikátor** deklarace identity s **userprincipalname** atribut, který obsahuje jedinečné ID uživatele, která budou odeslána do aplikace Beeline v každé úspěšné odpovědi SAML.</span><span class="sxs-lookup"><span data-stu-id="a961f-171">Here we have mapped the **User Identifier** claim with the **userprincipalname** attribute, which provides unique user ID, which will be sent to the Beeline application in the every successful SAML Response.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_attribute.png)  

6. <span data-ttu-id="a961f-173">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a961f-173">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a961f-175">Na **BeeLine konfigurace** klikněte na tlačítko **konfigurace BeeLine** otevřete **konfigurovat přihlášení** okno.</span><span class="sxs-lookup"><span data-stu-id="a961f-175">On the **BeeLine Configuration** section, click **Configure BeeLine** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a961f-176">Kopírování **Sign-Out URL** a **SAML Entity ID** z **Stručná referenční příručka části.**</span><span class="sxs-lookup"><span data-stu-id="a961f-176">Copy the **Sign-Out URL** and **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_configure.png) 

8. <span data-ttu-id="a961f-178">Konfigurace jednotného přihlašování na **BeeLine** straně, budete muset odeslat stažené **soubor XML s metadaty** a **SAML Entity ID**, **Sign-Out URL** k [tým podpory BeeLine](https://www.beeline.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="a961f-178">To configure single sign-on on **BeeLine** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID**, **Sign-Out URL** to [BeeLine support team](https://www.beeline.com/contact-us/).</span></span>

> [!TIP]
> <span data-ttu-id="a961f-179">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="a961f-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a961f-180">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="a961f-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a961f-181">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a961f-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a961f-182">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a961f-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="a961f-183">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a961f-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="a961f-185">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a961f-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a961f-186">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="a961f-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a961f-188">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="a961f-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a961f-190">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a961f-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a961f-192">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="a961f-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a961f-194">a.</span><span class="sxs-lookup"><span data-stu-id="a961f-194">a.</span></span> <span data-ttu-id="a961f-195">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a961f-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a961f-196">b.</span><span class="sxs-lookup"><span data-stu-id="a961f-196">b.</span></span> <span data-ttu-id="a961f-197">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a961f-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a961f-198">c.</span><span class="sxs-lookup"><span data-stu-id="a961f-198">c.</span></span> <span data-ttu-id="a961f-199">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="a961f-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a961f-200">d.</span><span class="sxs-lookup"><span data-stu-id="a961f-200">d.</span></span> <span data-ttu-id="a961f-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a961f-201">Click **Create**.</span></span>
 
### <a name="creating-a-beeline-test-user"></a><span data-ttu-id="a961f-202">Vytvoření zkušebního uživatele BeeLine</span><span class="sxs-lookup"><span data-stu-id="a961f-202">Creating a BeeLine test user</span></span>

<span data-ttu-id="a961f-203">V této části vytvoříte volal Britta Simon v Beeline uživatele.</span><span class="sxs-lookup"><span data-stu-id="a961f-203">In this section, you create a user called Britta Simon in Beeline.</span></span> <span data-ttu-id="a961f-204">Beeline aplikace, musí všichni uživatelé zřídit v aplikaci před provedením jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="a961f-204">Beeline application needs all the users to be provisioned in the application before doing Single Sign On.</span></span> <span data-ttu-id="a961f-205">Proto práce s [tým podpory BeeLine](https://www.beeline.com/contact-us/) ke zřízení tito uživatelé do aplikace.</span><span class="sxs-lookup"><span data-stu-id="a961f-205">So work with the [BeeLine support team](https://www.beeline.com/contact-us/) to provision all these users into the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a961f-206">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="a961f-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a961f-207">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BeeLine.</span><span class="sxs-lookup"><span data-stu-id="a961f-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BeeLine.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="a961f-209">**Pokud chcete přiřadit Britta Simon BeeLine, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="a961f-209">**To assign Britta Simon to BeeLine, perform the following steps:**</span></span>

1. <span data-ttu-id="a961f-210">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="a961f-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="a961f-212">V seznamu aplikací vyberte **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="a961f-212">In the applications list, select **BeeLine**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_app.png) 

3. <span data-ttu-id="a961f-214">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="a961f-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="a961f-216">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="a961f-216">Click **Add** button.</span></span> <span data-ttu-id="a961f-217">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a961f-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="a961f-219">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="a961f-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a961f-220">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a961f-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a961f-221">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="a961f-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a961f-222">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="a961f-222">Testing single sign-on</span></span>

<span data-ttu-id="a961f-223">V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="a961f-223">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="a961f-224">Když kliknete na dlaždici Beeline na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Beeline.</span><span class="sxs-lookup"><span data-stu-id="a961f-224">When you click the Beeline tile in the Access Panel, you should get automatically signed-on to your Beeline application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a961f-225">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a961f-225">Additional resources</span></span>

* [<span data-ttu-id="a961f-226">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a961f-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a961f-227">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a961f-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png

