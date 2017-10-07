---
title: "aaaAuthorize vývojářským účtům pomocí Azure Active Directory – Azure API Management | Microsoft Docs"
description: "Zjistěte, jak tooauthorize uživatelů pomocí služby Azure Active Directory ve službě API Management."
services: api-management
documentationcenter: API Management
author: steved0x
manager: erikre
editor: 
ms.assetid: 33a69a83-94f2-4e4e-9cef-f2a5af3c9732
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: ebf5447a509a47df35e4262138bfcf423cb1dd5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooauthorize-developer-accounts-using-azure-active-directory-in-azure-api-management"></a><span data-ttu-id="3e8f7-103">Jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory v Azure API Management</span><span class="sxs-lookup"><span data-stu-id="3e8f7-103">How tooauthorize developer accounts using Azure Active Directory in Azure API Management</span></span>
## <a name="overview"></a><span data-ttu-id="3e8f7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="3e8f7-104">Overview</span></span>
<span data-ttu-id="3e8f7-105">Tento průvodce vám ukáže, jak tooenable přístup k portálu pro vývojáře toohello pro uživatele ze služby Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-105">This guide shows you how tooenable access toohello developer portal for users from Azure Active Directory.</span></span> <span data-ttu-id="3e8f7-106">Tento průvodce také ukazuje, jak toomanage skupiny uživatelů Azure Active Directory tak, že přidáte externí skupiny, které obsahují hello uživatelů Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-106">This guide also shows you how toomanage groups of Azure Active Directory users by adding external groups that contain hello users of an Azure Active Directory.</span></span>

> <span data-ttu-id="3e8f7-107">toocomplete hello kroky v této příručce je nejdřív potřeba Azure Active Directory které toocreate aplikace.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-107">toocomplete hello steps in this guide you must first have an Azure Active Directory in which toocreate an application.</span></span>
> 
> 

## <a name="how-tooauthorize-developer-accounts-using-azure-active-directory"></a><span data-ttu-id="3e8f7-108">Jak tooauthorize vývojáře účtů pomocí služby Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e8f7-108">How tooauthorize developer accounts using Azure Active Directory</span></span>
<span data-ttu-id="3e8f7-109">tooget začít, klikněte na tlačítko **portál vydavatele** v hello portál Azure pro služby API Management.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-109">tooget started, click **Publisher portal** in hello Azure portal for your API Management service.</span></span> <span data-ttu-id="3e8f7-110">Tím přejdete portál vydavatele toohello API Management.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-110">This takes you toohello API Management publisher portal.</span></span>

![Portál vydavatele][api-management-management-console]

> <span data-ttu-id="3e8f7-112">Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v hello [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-112">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Get started with Azure API Management][Get started with Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="3e8f7-113">Klikněte na tlačítko **zabezpečení** z hello **API Management** nabídky na levé straně hello a klikněte na **externí identity**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-113">Click **Security** from hello **API Management** menu on hello left and click **External Identities**.</span></span>

![Externí identity][api-management-security-external-identities]

<span data-ttu-id="3e8f7-115">Klikněte na tlačítko **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-115">Click **Azure Active Directory**.</span></span> <span data-ttu-id="3e8f7-116">Poznamenejte si hello **adresy URL pro přesměrování** a přepnout tooyour Azure Active Directory v hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-116">Make a note of hello **Redirect URL** and switch over tooyour Azure Active Directory in hello Azure Classic Portal.</span></span>

![Externí identity][api-management-security-aad-new]

<span data-ttu-id="3e8f7-118">Klikněte na tlačítko hello **přidat** tlačítko toocreate novou aplikaci Azure Active Directory a vyberte **přidat aplikaci, kterou vyvíjí Moje organizace**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-118">Click hello **Add** button toocreate a new Azure Active Directory application, and choose **Add an application my organization is developing**.</span></span>

![Přidejte novou aplikaci Azure Active Directory][api-management-new-aad-application-menu]

<span data-ttu-id="3e8f7-120">Zadejte název aplikace hello, vyberte možnost **webové aplikace nebo webové rozhraní API**a klikněte na tlačítko Další hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-120">Enter a name for hello application, select **Web application and/or Web API**, and click hello next button.</span></span>

![Novou aplikaci Azure Active Directory][api-management-new-aad-application-1]

<span data-ttu-id="3e8f7-122">Pro **přihlašovací adresa URL**, zadejte hello přihlašování URL portálu pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-122">For **Sign-on URL**, enter hello sign-on URL of your developer portal.</span></span> <span data-ttu-id="3e8f7-123">V tomto příkladu hello **přihlašovací adresa URL** je `https://aad03.portal.current.int-azure-api.net/signin`.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-123">In this example, hello **Sign-on URL** is `https://aad03.portal.current.int-azure-api.net/signin`.</span></span> 

<span data-ttu-id="3e8f7-124">Pro hello **URL ID aplikace**, zadejte hello výchozí doménu nebo vlastní doménu pro hello Azure Active Directory a připojení tooit jedinečného řetězce.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-124">For hello **App ID URL**, enter either hello default domain or a custom domain for hello Azure Active Directory, and append a unique string tooit.</span></span> <span data-ttu-id="3e8f7-125">V tomto příkladu hello výchozí doménu **https://contoso5api.onmicrosoft.com** se používá s příponou hello **/api** zadaný.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-125">In this example, hello default domain of **https://contoso5api.onmicrosoft.com** is used with hello suffix of **/api** specified.</span></span>

![Nové vlastnosti aplikace Azure Active Directory][api-management-new-aad-application-2]

<span data-ttu-id="3e8f7-127">Klikněte na tlačítko hello zaškrtněte tlačítko toosave a vytvoření aplikace hello a přepínače toohello **konfigurace** kartě tooconfigure hello novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-127">Click hello check button toosave and create hello application, and switch toohello **Configure** tab tooconfigure hello new application.</span></span>

![Vytvořit novou aplikaci Azure Active Directory][api-management-new-aad-app-created]

<span data-ttu-id="3e8f7-129">Pokud se více Azure Active Directory se používá pro tuto aplikaci toobe, klikněte na tlačítko **Ano** pro **aplikace je víceklientské**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-129">If multiple Azure Active Directories are going toobe used for this application, click **Yes** for **Application is multi-tenant**.</span></span> <span data-ttu-id="3e8f7-130">Výchozí hodnota Hello je **ne**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-130">hello default is **No**.</span></span>

![Aplikace je více klientů][api-management-aad-app-multi-tenant]

<span data-ttu-id="3e8f7-132">Kopírování hello **adresy URL pro přesměrování** z hello **Azure Active Directory** části hello **externí identity** v portálu vydavatele hello a vložte jej do hello **Adresa URL odpovědi** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-132">Copy hello **Redirect URL** from hello **Azure Active Directory** section of hello **External Identities** tab in hello publisher portal and paste it into hello **Reply URL** text box.</span></span> 

![Adresa URL odpovědi][api-management-aad-reply-url]

<span data-ttu-id="3e8f7-134">Karta, vyberte hello konfigurace Scroll toohello dolní části hello **oprávnění aplikací** rozevíracího seznamu a zkontrolujte **čtení dat adresáře**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-134">Scroll toohello bottom of hello configure tab, select hello **Application Permissions** drop-down, and check **Read directory data**.</span></span>

![Oprávnění aplikací][api-management-aad-app-permissions]

<span data-ttu-id="3e8f7-136">Vyberte hello **delegování oprávnění** rozevíracího seznamu a zkontrolujte **povolit přihlášení a čtení uživatelských profilů**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-136">Select hello **Delegate Permissions** drop-down, and check **Enable sign-on and read users' profiles**.</span></span>

![Přidělená oprávnění][api-management-aad-delegated-permissions]

> <span data-ttu-id="3e8f7-138">Další informace o aplikaci a přidělená oprávnění najdete v tématu [hello přístup k rozhraní Graph API][Accessing hello Graph API].</span><span class="sxs-lookup"><span data-stu-id="3e8f7-138">For more information about application and delegated permissions, see [Accessing hello Graph API][Accessing hello Graph API].</span></span>
> 
> 

<span data-ttu-id="3e8f7-139">Kopírování hello **Id klienta** toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-139">Copy hello **Client Id** toohello clipboard.</span></span>

![Id klienta][api-management-aad-app-client-id]

<span data-ttu-id="3e8f7-141">Přepněte zpět toohello portál vydavatele a vložte hello **Id klienta** zkopírovaných z konfigurace aplikace hello Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-141">Switch back toohello publisher portal and paste in hello **Client Id** copied from hello Azure Active Directory application configuration.</span></span>

![Id klienta][api-management-client-id]

<span data-ttu-id="3e8f7-143">Přepnout zpět toohello konfigurace Azure Active Directory a klikněte na tlačítko hello **vyberte dobu trvání** rozevírací seznam v hello **klíče** části a zadat interval.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-143">Switch back toohello Azure Active Directory configuration, and click hello **Select duration** drop-down in hello **Keys** section and specify an interval.</span></span> <span data-ttu-id="3e8f7-144">V tomto příkladu **1 rok** se používá.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-144">In this example, **1 year** is used.</span></span>

![Klíč][api-management-aad-key-before-save]

<span data-ttu-id="3e8f7-146">Klikněte na tlačítko **Uložit** toosave hello konfiguraci a zobrazení hello klíč.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-146">Click **Save** toosave hello configuration and display hello key.</span></span> <span data-ttu-id="3e8f7-147">Kopírování hello klíče toohello schránky.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-147">Copy hello key toohello clipboard.</span></span>

> <span data-ttu-id="3e8f7-148">Tento klíč si poznamenejte.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-148">Make a note of this key.</span></span> <span data-ttu-id="3e8f7-149">Po zavření okna konfigurace Azure Active Directory hello hello klíč nelze zobrazit znovu.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-149">Once you close hello Azure Active Directory configuration window, hello key cannot be displayed again.</span></span>
> 
> 

![Klíč][api-management-aad-key-after-save]

<span data-ttu-id="3e8f7-151">Přepínač back toohello vydavatele portál a vložit hello klíč do hello **tajný klíč klienta** textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-151">Switch back toohello publisher portal and paste hello key into hello **Client Secret** text box.</span></span>

![Tajný klíč klienta][api-management-client-secret]

<span data-ttu-id="3e8f7-153">**Povolené klienty** Určuje adresáře, které mají přístup toohello rozhraní API pro instanci služby API Management hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-153">**Allowed Tenants** specifies which directories have access toohello APIs of hello API Management service instance.</span></span> <span data-ttu-id="3e8f7-154">Zadejte domény hello hello toowhich instancí Azure Active Directory má toogrant přístup.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-154">Specify hello domains of hello Azure Active Directory instances toowhich you want toogrant access.</span></span> <span data-ttu-id="3e8f7-155">Více domén můžete oddělit vložení znaků newline, mezery nebo čárkami.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-155">You can separate multiple domains with newlines, spaces, or commas.</span></span>

![Povolené klientů][api-management-client-allowed-tenants]


<span data-ttu-id="3e8f7-157">Jakmile hello potřeby zadána konfigurace, klikněte na možnost **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-157">Once hello desired configuration is specified, click **Save**.</span></span>

![Uložit][api-management-client-allowed-tenants-save]

<span data-ttu-id="3e8f7-159">Po uložení změn hello hello uživatele v hello zadaný Azure Active Directory můžete pomocí následujících kroků hello v přihlásit portál pro vývojáře toohello [přihlásit pomocí účtu Azure Active Directory portálu pro vývojáře toohello] [Log in toohello Developer portal using an Azure Active Directory account].</span><span class="sxs-lookup"><span data-stu-id="3e8f7-159">Once hello changes are saved, hello users in hello specified Azure Active Directory can sign in toohello Developer portal by following hello steps in [Log in toohello Developer portal using an Azure Active Directory account][Log in toohello Developer portal using an Azure Active Directory account].</span></span>

<span data-ttu-id="3e8f7-160">V hello lze zadat více domén **klientům povoleno** části.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-160">Multiple domains can be specified in hello **Allowed Tenants** section.</span></span> <span data-ttu-id="3e8f7-161">Před každý uživatel může přihlásit z jiné doméně než hello původní domény, kde byl zaregistrován hello aplikace, musí se globální správce jinou doménu hello udělit oprávnění tooaccess aplikace hello dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-161">Before any user can log in from a different domain than hello original domain where hello application was registered, a global administrator of hello different domain must grant permission for hello application tooaccess directory data.</span></span> <span data-ttu-id="3e8f7-162">toogrant oprávnění globálního správce hello by měli přejít příliš`https://<URL of your developer portal>/aadadminconsent` (například https://contoso.portal.azure-api.net/aadadminconsent), zadejte název domény hello hello chtějí toogive přístup tooand klienta služby Active Directory, klikněte na tlačítko Odeslat.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-162">toogrant permission, hello global administrator should go too`https://<URL of your developer portal>/aadadminconsent` (for example, https://contoso.portal.azure-api.net/aadadminconsent), type in hello domain name of hello Active Directory tenant they want toogive access tooand click Submit.</span></span> <span data-ttu-id="3e8f7-163">V následující hello příklad, globální správce `miaoaad.onmicrosoft.com` se pokouší toogive oprávnění toothis konkrétní developer portal.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-163">In hello following example, a global administrator from `miaoaad.onmicrosoft.com` is trying toogive permission toothis particular developer portal.</span></span> 

![Oprávnění][api-management-aad-consent]

<span data-ttu-id="3e8f7-165">Na další obrazovce hello bude globální správce hello výzvami tooconfirm udělíte oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-165">In hello next screen, hello global administrator will be prompted tooconfirm giving hello permission.</span></span> 

![Oprávnění][api-management-permissions-form]

> <span data-ttu-id="3e8f7-167">Pokud není globální správce pokusí toolog v před oprávnění jsou udělena globální správce, hello pokus o přihlášení selže, se zobrazí chybové obrazovce.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-167">If a non-global administrator tries toolog in before permissions are granted by a global administrator, hello login attempt fails and an error screen is displayed.</span></span>
> 
> 

## <a name="how-tooadd-an-external-azure-active-directory-group"></a><span data-ttu-id="3e8f7-168">Jak tooadd externí Azure Active Directory skupiny</span><span class="sxs-lookup"><span data-stu-id="3e8f7-168">How tooadd an external Azure Active Directory Group</span></span>
<span data-ttu-id="3e8f7-169">Když povolíte přístup pro uživatele v Azure Active Directory, můžete přidat, skupiny Azure Active Directory do rozhraní API správy toomore snadno spravovat hello přidružení hello vývojářů ve skupině hello hello požadovaných produktů.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-169">After enabling access for users in an Azure Active Directory, you can add Azure Active Directory groups into API Management toomore easily manage hello association of hello developers in hello group with hello desired products.</span></span>

> <span data-ttu-id="3e8f7-170">tooconfigure externí skupiny Azure Active Directory, hello Azure Active Directory musí nejdřív nakonfigurovat na kartě hello identit pomocí následujícího postupu hello v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-170">tooconfigure an external Azure Active Directory group, hello Azure Active Directory must first be configured in hello Identities tab by following hello procedure in hello previous section.</span></span> 
> 
> 

<span data-ttu-id="3e8f7-171">Externí skupiny Azure Active Directory se přidají hello **viditelnost** kartě hello produktů, pro kterou chcete toogrant přístup toohello skupiny.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-171">External Azure Active Directory groups are added from hello **Visibility** tab of hello product for which you wish toogrant access toohello group.</span></span> <span data-ttu-id="3e8f7-172">Klikněte na tlačítko **produkty**a potom klikněte na název hello požadované produktu hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-172">Click **Products**, and then click hello name of hello desired product.</span></span>

![Konfigurace produktu][api-management-configure-product]

<span data-ttu-id="3e8f7-174">Přepínač toohello **viditelnost** a klikněte na **přidat skupiny z Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-174">Switch toohello **Visibility** tab, and click **Add Groups from Azure Active Directory**.</span></span>

![Přidání skupin][api-management-add-groups]

<span data-ttu-id="3e8f7-176">Vyberte hello **klienta Azure Active Directory** z hello rozevíracího seznamu a potom název typu hello hello požadované skupiny v hello **skupiny** toobe přidat textové pole.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-176">Select hello **Azure Active Directory Tenant** from hello drop-down list, and then type hello name of hello desired group in hello **Groups** toobe added text box.</span></span>

![Vyberte skupinu][api-management-select-group]

<span data-ttu-id="3e8f7-178">Tento název skupiny lze nalézt v hello **skupiny** seznam pro Azure Active Directory, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-178">This group name can be found in hello **Groups** list for your Azure Active Directory, as shown in hello following example.</span></span>

![Seznam skupin Azure Active Directory][api-management-aad-groups-list]

<span data-ttu-id="3e8f7-180">Klikněte na tlačítko **přidat** toovalidate hello název skupiny a přidejte skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-180">Click **Add** toovalidate hello group name and add hello group.</span></span> <span data-ttu-id="3e8f7-181">V tomto příkladu hello **Contoso 5 vývojáři** je přidána externí skupina.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-181">In this example, hello **Contoso 5 Developers** external group is added.</span></span> 

![Přidat skupinu][api-management-aad-group-added]

<span data-ttu-id="3e8f7-183">Klikněte na tlačítko **Uložit** toosave hello výběr nové skupiny.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-183">Click **Save** toosave hello new group selection.</span></span>

<span data-ttu-id="3e8f7-184">Jakmile skupinu služby Azure Active Directory byla nakonfigurována jedním z produktů, je k dispozici toobe na hello zaškrtnuta možnost **viditelnost** hello kartu pro ostatní produkty v instanci služby API Management hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-184">Once an Azure Active Directory group has been configured from one product, it is available toobe checked on hello **Visibility** tab for hello other products in hello API Management service instance.</span></span>

<span data-ttu-id="3e8f7-185">tooreview a konfigurovat hello vlastnosti pro externí skupiny po byly přidány, klikněte na název hello skupiny hello z hello **skupiny** kartě.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-185">tooreview and configure hello properties for external groups once they have been added, click hello name of hello group from hello **Groups** tab.</span></span>

![Správa skupin][api-management-groups]

<span data-ttu-id="3e8f7-187">Zde můžete upravit hello **název** a hello **popis** hello skupiny.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-187">From here you can edit hello **Name** and hello **Description** of hello group.</span></span>

![Úprava skupiny][api-management-edit-group]

<span data-ttu-id="3e8f7-189">Uživatelé z hello nakonfigurované Azure Active Directory můžete přihlásit na portál pro vývojáře toohello a zobrazení a přihlášení k odběru tooany skupin, ke kterým mají viditelnost podle následujících pokynů hello v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-189">Users from hello configured Azure Active Directory can sign in toohello Developer portal and view and subscribe tooany groups for which they have visibility by following hello instructions in hello following section.</span></span>

## <a name="how-toolog-in-toohello-developer-portal-using-an-azure-active-directory-account"></a><span data-ttu-id="3e8f7-190">Jak toolog v portálu pro vývojáře toohello pomocí účtu Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e8f7-190">How toolog in toohello Developer portal using an Azure Active Directory account</span></span>
<span data-ttu-id="3e8f7-191">toolog do portálu pro vývojáře hello pomocí účtu Azure Active Directory nakonfigurované v předchozích částech hello, otevřete nové okno prohlížeče pomocí hello **přihlašovací adresa URL** z konfigurace aplikace hello služby Active Directory a klikněte na **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-191">toolog into hello Developer portal using an Azure Active Directory account configured in hello previous sections, open a new browser window using hello **Sign-on URL** from hello Active Directory application configuration, and click **Azure Active Directory**.</span></span>

![Portál pro vývojáře][api-management-dev-portal-signin]

<span data-ttu-id="3e8f7-193">Zadejte přihlašovací údaje hello jednoho hello uživatelů ve vašem Azure Active Directory a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-193">Enter hello credentials of one of hello users in your Azure Active Directory, and click **Sign in**.</span></span>

![Přihlášení][api-management-aad-signin]

<span data-ttu-id="3e8f7-195">S registračním formuláři můžete být vyzváni, pokud je vyžadováno žádné další údaje.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-195">You may be prompted with a registration form if any additional information is required.</span></span> <span data-ttu-id="3e8f7-196">Dokončení hello registračním formuláři a klikněte na tlačítko **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-196">Complete hello registration form and click **Sign up**.</span></span>

![Registrace][api-management-complete-registration]

<span data-ttu-id="3e8f7-198">Uživatel je nyní přihlášeni hello portál pro vývojáře pro instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="3e8f7-198">Your user is now logged into hello developer portal for your API Management service instance.</span></span>

![Registrace je dokončena.][api-management-registration-complete]

[api-management-management-console]: ./media/api-management-howto-aad/api-management-management-console.png
[api-management-security-external-identities]: ./media/api-management-howto-aad/api-management-security-external-identities.png
[api-management-security-aad-new]: ./media/api-management-howto-aad/api-management-security-aad-new.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-aad/api-management-new-aad-application-menu.png
[api-management-new-aad-application-1]: ./media/api-management-howto-aad/api-management-new-aad-application-1.png
[api-management-new-aad-application-2]: ./media/api-management-howto-aad/api-management-new-aad-application-2.png
[api-management-new-aad-app-created]: ./media/api-management-howto-aad/api-management-new-aad-app-created.png
[api-management-aad-app-permissions]: ./media/api-management-howto-aad/api-management-aad-app-permissions.png
[api-management-aad-app-client-id]: ./media/api-management-howto-aad/api-management-aad-app-client-id.png
[api-management-client-id]: ./media/api-management-howto-aad/api-management-client-id.png
[api-management-aad-key-before-save]: ./media/api-management-howto-aad/api-management-aad-key-before-save.png
[api-management-aad-key-after-save]: ./media/api-management-howto-aad/api-management-aad-key-after-save.png
[api-management-client-secret]: ./media/api-management-howto-aad/api-management-client-secret.png
[api-management-client-allowed-tenants]: ./media/api-management-howto-aad/api-management-client-allowed-tenants.png
[api-management-client-allowed-tenants-save]: ./media/api-management-howto-aad/api-management-client-allowed-tenants-save.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-aad/api-management-aad-delegated-permissions.png
[api-management-dev-portal-signin]: ./media/api-management-howto-aad/api-management-dev-portal-signin.png
[api-management-aad-signin]: ./media/api-management-howto-aad/api-management-aad-signin.png
[api-management-complete-registration]: ./media/api-management-howto-aad/api-management-complete-registration.png
[api-management-registration-complete]: ./media/api-management-howto-aad/api-management-registration-complete.png
[api-management-aad-app-multi-tenant]: ./media/api-management-howto-aad/api-management-aad-app-multi-tenant.png
[api-management-aad-reply-url]: ./media/api-management-howto-aad/api-management-aad-reply-url.png
[api-management-aad-consent]: ./media/api-management-howto-aad/api-management-aad-consent.png
[api-management-permissions-form]: ./media/api-management-howto-aad/api-management-permissions-form.png
[api-management-configure-product]: ./media/api-management-howto-aad/api-management-configure-product.png
[api-management-add-groups]: ./media/api-management-howto-aad/api-management-add-groups.png
[api-management-select-group]: ./media/api-management-howto-aad/api-management-select-group.png
[api-management-aad-groups-list]: ./media/api-management-howto-aad/api-management-aad-groups-list.png
[api-management-aad-group-added]: ./media/api-management-howto-aad/api-management-aad-group-added.png
[api-management-groups]: ./media/api-management-howto-aad/api-management-groups.png
[api-management-edit-group]: ./media/api-management-howto-aad/api-management-edit-group.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: api-management-get-started.md#create-service-instance

[http://oauth.net/2/]: http://oauth.net/2/
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[Accessing hello Graph API]: http://msdn.microsoft.com/library/azure/dn132599.aspx#BKMK_Graph

[Prerequisites]: #prerequisites
[Configure an OAuth 2.0 authorization server in API Management]: #step1
[Configure an API toouse OAuth 2.0 user authorization]: #step2
[Test hello OAuth 2.0 user authorization in hello Developer Portal]: #step3
[Next steps]: #next-steps

[Log in toohello Developer portal using an Azure Active Directory account]: #Log-in-to-the-Developer-portal-using-an-Azure-Active-Directory-account

