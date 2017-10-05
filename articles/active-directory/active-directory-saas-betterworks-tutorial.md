---
title: 'Kurz: Azure Active Directory integrace s BetterWorks | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a BetterWorks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5bb9505a-be02-46ae-9979-5308715d2b47
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: d6a5b167c0befbd0fe2c65bdd16abc35ed0a659c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-betterworks"></a><span data-ttu-id="f78ec-103">Kurz: Azure Active Directory integrace s BetterWorks</span><span class="sxs-lookup"><span data-stu-id="f78ec-103">Tutorial: Azure Active Directory integration with BetterWorks</span></span>

<span data-ttu-id="f78ec-104">V tomto kurzu zjistěte, jak integrovat BetterWorks s Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f78ec-104">In this tutorial, you learn how to integrate BetterWorks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f78ec-105">Integrace BetterWorks s Azure AD poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="f78ec-105">Integrating BetterWorks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f78ec-106">Můžete řídit ve službě Azure AD, který má přístup k BetterWorks</span><span class="sxs-lookup"><span data-stu-id="f78ec-106">You can control in Azure AD who has access to BetterWorks</span></span>
- <span data-ttu-id="f78ec-107">Můžete povolit uživatelům, aby automaticky získat přihlášení k BetterWorks (jednotné přihlášení) s jejich účty Azure AD</span><span class="sxs-lookup"><span data-stu-id="f78ec-107">You can enable your users to automatically get signed-on to BetterWorks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f78ec-108">Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f78ec-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f78ec-109">Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f78ec-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f78ec-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f78ec-110">Prerequisites</span></span>

<span data-ttu-id="f78ec-111">Konfigurace integrace Azure AD s BetterWorks, potřebujete následující položky:</span><span class="sxs-lookup"><span data-stu-id="f78ec-111">To configure Azure AD integration with BetterWorks, you need the following items:</span></span>

- <span data-ttu-id="f78ec-112">Předplatné služby Azure AD</span><span class="sxs-lookup"><span data-stu-id="f78ec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f78ec-113">BetterWorks jednotného přihlašování povolené předplatné</span><span class="sxs-lookup"><span data-stu-id="f78ec-113">A BetterWorks single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f78ec-114">K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f78ec-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f78ec-115">Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:</span><span class="sxs-lookup"><span data-stu-id="f78ec-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f78ec-116">Nepoužívejte provozním prostředí, pokud to není nutné.</span><span class="sxs-lookup"><span data-stu-id="f78ec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f78ec-117">Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební verze jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f78ec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f78ec-118">Popis scénáře</span><span class="sxs-lookup"><span data-stu-id="f78ec-118">Scenario description</span></span>
<span data-ttu-id="f78ec-119">V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="f78ec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f78ec-120">Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:</span><span class="sxs-lookup"><span data-stu-id="f78ec-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f78ec-121">Přidání BetterWorks z Galerie</span><span class="sxs-lookup"><span data-stu-id="f78ec-121">Adding BetterWorks from the gallery</span></span>
2. <span data-ttu-id="f78ec-122">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f78ec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-betterworks-from-the-gallery"></a><span data-ttu-id="f78ec-123">Přidání BetterWorks z Galerie</span><span class="sxs-lookup"><span data-stu-id="f78ec-123">Adding BetterWorks from the gallery</span></span>
<span data-ttu-id="f78ec-124">Při konfiguraci integrace BetterWorks do služby Azure AD musíte přidat do seznamu spravovaných aplikací SaaS BetterWorks z galerie.</span><span class="sxs-lookup"><span data-stu-id="f78ec-124">To configure the integration of BetterWorks into Azure AD, you need to add BetterWorks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f78ec-125">**Pokud chcete přidat BetterWorks z galerie, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f78ec-125">**To add BetterWorks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f78ec-126">V  **[portál Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f78ec-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f78ec-128">Přejděte na **podnikové aplikace, které**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f78ec-129">Pak přejděte na **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-129">Then go to **All applications**.</span></span>

    ![Aplikace][2]
    
3. <span data-ttu-id="f78ec-131">Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f78ec-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Aplikace][3]

4. <span data-ttu-id="f78ec-133">Do vyhledávacího pole zadejte **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-133">In the search box, type **BetterWorks**.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_search.png)

5. <span data-ttu-id="f78ec-135">Na panelu výsledků vyberte **BetterWorks**a potom klikněte na **přidat** tlačítko Přidat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f78ec-135">In the results panel, select **BetterWorks**, and then click **Add** button to add the application.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f78ec-137">Konfigurace a testování Azure AD jednotného přihlašování</span><span class="sxs-lookup"><span data-stu-id="f78ec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f78ec-138">V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s BetterWorks podle testovacího uživatele názvem "Britta Simon."</span><span class="sxs-lookup"><span data-stu-id="f78ec-138">In this section, you configure and test Azure AD single sign-on with BetterWorks based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f78ec-139">Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v BetterWorks je pro uživatele ve službě Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f78ec-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BetterWorks is to a user in Azure AD.</span></span> <span data-ttu-id="f78ec-140">Jinými slovy odkaz vztah mezi uživatele Azure AD a související uživatelské v BetterWorks musí navázat.</span><span class="sxs-lookup"><span data-stu-id="f78ec-140">In other words, a link relationship between an Azure AD user and the related user in BetterWorks needs to be established.</span></span>

<span data-ttu-id="f78ec-141">V BetterWorks, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** k navázání vztahu odkazu.</span><span class="sxs-lookup"><span data-stu-id="f78ec-141">In BetterWorks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f78ec-142">Nakonfigurovat a otestovat Azure AD jednotné přihlašování s BetterWorks, je třeba dokončit následující stavební bloky:</span><span class="sxs-lookup"><span data-stu-id="f78ec-142">To configure and test Azure AD single sign-on with BetterWorks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f78ec-143">**[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.</span><span class="sxs-lookup"><span data-stu-id="f78ec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f78ec-144">**[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f78ec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f78ec-145">**[Vytvoření zkušebního uživatele BetterWorks](#creating-a-betterworks-test-user)**  – Pokud chcete mít protějšek Britta Simon v BetterWorks propojeném s Azure AD reprezentace daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="f78ec-145">**[Creating a BetterWorks test user](#creating-a-betterworks-test-user)** - to have a counterpart of Britta Simon in BetterWorks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f78ec-146">**[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f78ec-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f78ec-147">**[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f78ec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f78ec-148">Konfigurace Azure AD jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f78ec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f78ec-149">V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="f78ec-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BetterWorks application.</span></span>

<span data-ttu-id="f78ec-150">**Ke konfiguraci Azure AD jednotné přihlašování s BetterWorks, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f78ec-150">**To configure Azure AD single sign-on with BetterWorks, perform the following steps:**</span></span>

1. <span data-ttu-id="f78ec-151">Na portálu Azure na **BetterWorks** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-151">In the Azure portal, on the **BetterWorks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurovat jednotné přihlašování][4]

2. <span data-ttu-id="f78ec-153">Na **jednotného přihlašování** dialogovém okně, vyberte **režimu** jako **na základě SAML přihlašování** umožňující jednotného přihlašování.</span><span class="sxs-lookup"><span data-stu-id="f78ec-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_samlbase.png)

3. <span data-ttu-id="f78ec-155">Na **BetterWorks domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **IDP iniciované režimu**:</span><span class="sxs-lookup"><span data-stu-id="f78ec-155">On the **BetterWorks Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**:</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url.png)

    <span data-ttu-id="f78ec-157">a.</span><span class="sxs-lookup"><span data-stu-id="f78ec-157">a.</span></span> <span data-ttu-id="f78ec-158">V **identifikátor** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://app.betterworks.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="f78ec-158">In the **Identifier** textbox, type a URL using the following pattern: `https://app.betterworks.com/saml2/metadata/`</span></span>

    <span data-ttu-id="f78ec-159">b.</span><span class="sxs-lookup"><span data-stu-id="f78ec-159">b.</span></span> <span data-ttu-id="f78ec-160">V **adresa URL odpovědi** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://app.betterworks.com/saml2/acs/`</span><span class="sxs-lookup"><span data-stu-id="f78ec-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://app.betterworks.com/saml2/acs/`</span></span>

4. <span data-ttu-id="f78ec-161">Na **BetterWorks domény a adresy URL** část, pokud chcete nakonfigurovat aplikace **SP iniciované režimu**, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f78ec-161">On the **BetterWorks Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_url1.png)

    <span data-ttu-id="f78ec-163">a.</span><span class="sxs-lookup"><span data-stu-id="f78ec-163">a.</span></span> <span data-ttu-id="f78ec-164">Klikněte na **zobrazit upřesňující nastavení adresy URL**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-164">Click on the **Show advanced URL settings**.</span></span>

    <span data-ttu-id="f78ec-165">b.</span><span class="sxs-lookup"><span data-stu-id="f78ec-165">b.</span></span> <span data-ttu-id="f78ec-166">V **přihlašovací adresa URL** textovému poli, zadejte adresu URL pomocí následujícího vzorce:`https://app.betterworks.com`</span><span class="sxs-lookup"><span data-stu-id="f78ec-166">In the **Sign On URL** textbox, type a URL using the following pattern: `https://app.betterworks.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f78ec-167">Tyto nejsou skutečné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f78ec-167">These are not real values.</span></span> <span data-ttu-id="f78ec-168">Tyto hodnoty aktualizujte se adresa URL odpovědi, identifikátor a skutečný přihlašovací adresa URL.</span><span class="sxs-lookup"><span data-stu-id="f78ec-168">Update these values with the Reply URL, Identifier and actual Sign On URL.</span></span> <span data-ttu-id="f78ec-169">Obraťte se na [tým podpory BetterWorks](mailto:support@betterworks.com) k získání těchto hodnot.</span><span class="sxs-lookup"><span data-stu-id="f78ec-169">Contact [BetterWorks support team](mailto:support@betterworks.com) to get these values.</span></span>
 
4. <span data-ttu-id="f78ec-170">Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a potom uložte soubor metadat ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="f78ec-170">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_certificate.png)  

5. <span data-ttu-id="f78ec-172">Aplikace BetterWorks očekává SAML kontrolní výrazy ve specifickém formátu.</span><span class="sxs-lookup"><span data-stu-id="f78ec-172">BetterWorks application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="f78ec-173">Nakonfigurujte následující deklarace identity pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f78ec-173">Configure the following claims for this application.</span></span> <span data-ttu-id="f78ec-174">Můžete spravovat hodnoty těchto atributů z "**atribut**" aplikace.</span><span class="sxs-lookup"><span data-stu-id="f78ec-174">You can manage the values of these attributes from the "**Attribute**" tab of the application.</span></span> <span data-ttu-id="f78ec-175">Následující snímek obrazovky ukazuje příklad pro tento.</span><span class="sxs-lookup"><span data-stu-id="f78ec-175">The following screenshot shows an example for this.</span></span> 

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_attribute.png)

6. <span data-ttu-id="f78ec-177">Na **atributy tokenu SAML** dialogu pro každý řádek v tabulce níže, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f78ec-177">On the **SAML token attributes** dialog, for each row shown in the table below, perform the following steps:</span></span>
 
   | <span data-ttu-id="f78ec-178">Název atributu</span><span class="sxs-lookup"><span data-stu-id="f78ec-178">Attribute Name</span></span> | <span data-ttu-id="f78ec-179">Hodnota atributu</span><span class="sxs-lookup"><span data-stu-id="f78ec-179">Attribute Value</span></span> |
   | -------------- |  ------------ |
   | <span data-ttu-id="f78ec-180">saml_token</span><span class="sxs-lookup"><span data-stu-id="f78ec-180">saml_token</span></span>     | <span data-ttu-id="f78ec-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span><span class="sxs-lookup"><span data-stu-id="f78ec-181">bd189cf6-1701-11e6-8f90-d26992eca2a5</span></span> |

   <span data-ttu-id="f78ec-182">a.</span><span class="sxs-lookup"><span data-stu-id="f78ec-182">a.</span></span> <span data-ttu-id="f78ec-183">Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f78ec-183">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_04.png)

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_officespace_05.png)

   <span data-ttu-id="f78ec-186">b.</span><span class="sxs-lookup"><span data-stu-id="f78ec-186">b.</span></span> <span data-ttu-id="f78ec-187">V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="f78ec-187">In the **Name** textbox, type the attribute name shown for that row.</span></span> 

   <span data-ttu-id="f78ec-188">c.</span><span class="sxs-lookup"><span data-stu-id="f78ec-188">c.</span></span> <span data-ttu-id="f78ec-189">Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.</span><span class="sxs-lookup"><span data-stu-id="f78ec-189">From the **Value** list, type the attribute value shown for that row.</span></span>
    
   <span data-ttu-id="f78ec-190">d.</span><span class="sxs-lookup"><span data-stu-id="f78ec-190">d.</span></span> <span data-ttu-id="f78ec-191">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-191">Click **Ok**.</span></span>

7. <span data-ttu-id="f78ec-192">Klikněte na tlačítko **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f78ec-192">Click **Save** button.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f78ec-194">Konfigurace jednotného přihlašování na **BetterWorks** straně, budete muset odeslat stažené **soubor XML s metadaty** k [tým podpory BetterWorks](mailto:support@betterworks.com).</span><span class="sxs-lookup"><span data-stu-id="f78ec-194">To configure single sign-on on **BetterWorks** side, you need to send the downloaded **Metadata XML** to [BetterWorks support team](mailto:support@betterworks.com).</span></span>


> [!TIP]
> <span data-ttu-id="f78ec-195">Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com), zatímco nastavujete aplikace!</span><span class="sxs-lookup"><span data-stu-id="f78ec-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f78ec-196">Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** jednoduše klikněte na položku **jednotné přihlašování** kartě a přístup v embedded dokumentaci prostřednictvím **konfigurace** v dolní části.</span><span class="sxs-lookup"><span data-stu-id="f78ec-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f78ec-197">Můžete přečíst další informace o funkci embedded dokumentace: [vložených dokumentace k Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f78ec-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f78ec-198">Vytváření testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f78ec-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="f78ec-199">Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f78ec-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Vytvořit uživatele Azure AD][100]

<span data-ttu-id="f78ec-201">**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f78ec-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f78ec-202">V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.</span><span class="sxs-lookup"><span data-stu-id="f78ec-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f78ec-204">Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f78ec-206">Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** horní dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f78ec-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f78ec-208">Na **uživatele** dialogové okno stránky, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f78ec-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-betterworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f78ec-210">a.</span><span class="sxs-lookup"><span data-stu-id="f78ec-210">a.</span></span> <span data-ttu-id="f78ec-211">V **název** textovému poli, typ **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f78ec-212">b.</span><span class="sxs-lookup"><span data-stu-id="f78ec-212">b.</span></span> <span data-ttu-id="f78ec-213">V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f78ec-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f78ec-214">c.</span><span class="sxs-lookup"><span data-stu-id="f78ec-214">c.</span></span> <span data-ttu-id="f78ec-215">Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f78ec-216">d.</span><span class="sxs-lookup"><span data-stu-id="f78ec-216">d.</span></span> <span data-ttu-id="f78ec-217">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-217">Click **Create**.</span></span>
 
### <a name="creating-a-betterworks-test-user"></a><span data-ttu-id="f78ec-218">Vytvoření zkušebního uživatele BetterWorks</span><span class="sxs-lookup"><span data-stu-id="f78ec-218">Creating a BetterWorks test user</span></span>

<span data-ttu-id="f78ec-219">V této části vytvoříte volal Britta Simon v BetterWorks uživatele.</span><span class="sxs-lookup"><span data-stu-id="f78ec-219">In this section, you create a user called Britta Simon in BetterWorks.</span></span> <span data-ttu-id="f78ec-220">Práce s [tým podpory BetterWorks](mailto:support@betterworks.com) přidat uživatele do BetterWorks platformy.</span><span class="sxs-lookup"><span data-stu-id="f78ec-220">Work with [BetterWorks support team](mailto:support@betterworks.com) to add the users in the BetterWorks platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f78ec-221">Přiřazení testovacího uživatele Azure AD</span><span class="sxs-lookup"><span data-stu-id="f78ec-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f78ec-222">V této části povolíte Britta Simon používat Azure jednotné přihlašování pomocí udělení přístupu BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="f78ec-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BetterWorks.</span></span>

![Přiřadit uživatele][200] 

<span data-ttu-id="f78ec-224">**Pokud chcete přiřadit Britta Simon BetterWorks, proveďte následující kroky:**</span><span class="sxs-lookup"><span data-stu-id="f78ec-224">**To assign Britta Simon to BetterWorks, perform the following steps:**</span></span>

1. <span data-ttu-id="f78ec-225">Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Přiřadit uživatele][201] 

2. <span data-ttu-id="f78ec-227">V seznamu aplikací vyberte **BetterWorks**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-227">In the applications list, select **BetterWorks**.</span></span>

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-betterworks-tutorial/tutorial_betterworks_app.png) 

3. <span data-ttu-id="f78ec-229">V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.</span><span class="sxs-lookup"><span data-stu-id="f78ec-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Přiřadit uživatele][202] 

4. <span data-ttu-id="f78ec-231">Klikněte na tlačítko **přidat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f78ec-231">Click **Add** button.</span></span> <span data-ttu-id="f78ec-232">Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f78ec-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Přiřadit uživatele][203]

5. <span data-ttu-id="f78ec-234">Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.</span><span class="sxs-lookup"><span data-stu-id="f78ec-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f78ec-235">Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f78ec-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f78ec-236">Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="f78ec-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f78ec-237">Testování jednotné přihlašování</span><span class="sxs-lookup"><span data-stu-id="f78ec-237">Testing single sign-on</span></span>

<span data-ttu-id="f78ec-238">Cílem této části je Azure AD jeden přihlašování konfigurace pomocí přístupového panelu.</span><span class="sxs-lookup"><span data-stu-id="f78ec-238">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f78ec-239">Když kliknete na dlaždici BetterWorks na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci BetterWorks.</span><span class="sxs-lookup"><span data-stu-id="f78ec-239">When you click the BetterWorks tile in the Access Panel, you should get automatically signed-on to your BetterWorks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f78ec-240">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="f78ec-240">Additional resources</span></span>

* [<span data-ttu-id="f78ec-241">Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f78ec-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f78ec-242">Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f78ec-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-betterworks-tutorial/tutorial_general_203.png

