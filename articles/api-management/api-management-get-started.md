---
title: "aaaManage vašeho prvního rozhraní API v Azure API Management | Microsoft Docs"
description: "Zjistěte, jak toocreate rozhraní API, přidávat operace a začít pracovat s API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 51b7df8b-1c43-43c6-90c9-0aa24f48206b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7d43f33aa359c4d1e605e9fb41e43d323ca6a777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-first-api-in-azure-api-management"></a><span data-ttu-id="c58bb-103">Správa vašeho prvního rozhraní API ve službě Azure API Management</span><span class="sxs-lookup"><span data-stu-id="c58bb-103">Manage your first API in Azure API Management</span></span>
## <span data-ttu-id="c58bb-104"><a name="overview"></a>Přehled</span><span class="sxs-lookup"><span data-stu-id="c58bb-104"><a name="overview"> </a>Overview</span></span>
<span data-ttu-id="c58bb-105">Tento průvodce vám ukáže, jak tooquickly začít s používáním Azure API Management a vaše první volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-105">This guide shows you how tooquickly get started in using Azure API Management and make your first API call.</span></span>

## <span data-ttu-id="c58bb-106"><a name="concepts"></a>Co je Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="c58bb-106"><a name="concepts"> </a>What is Azure API Management?</span></span>
<span data-ttu-id="c58bb-107">Můžete použít jakýkoli back-end tootake Azure API Management a spustit plnohodnotný program API založené na něm.</span><span class="sxs-lookup"><span data-stu-id="c58bb-107">You can use Azure API Management tootake any backend and launch a full-fledged API program based on it.</span></span>

<span data-ttu-id="c58bb-108">Mezi obvyklé scénáře patří:</span><span class="sxs-lookup"><span data-stu-id="c58bb-108">Common scenarios include:</span></span>

* <span data-ttu-id="c58bb-109">**Zabezpečení mobilní infrastruktury** pomocí přístupu s klíči rozhraní API prostřednictvím brány, ochrana před útoky DOS pomocí omezování nebo používání rozšířených zásad zabezpečení, například ověřování tokenů JWT.</span><span class="sxs-lookup"><span data-stu-id="c58bb-109">**Securing mobile infrastructure** by gating access with API keys, preventing DOS attacks by using throttling, or using advanced security policies like JWT token validation.</span></span>
* <span data-ttu-id="c58bb-110">**Povolování ekosystémů partnera** prostřednictvím nabídky rychlého připojení partnera skrze hello vývojáře portál a vytváření rozhraní API průčelí za toodecouple od interních implementací, které nejsou zralé k partner využíval tomu.</span><span class="sxs-lookup"><span data-stu-id="c58bb-110">**Enabling ISV partner ecosystems** by offering fast partner onboarding through hello developer portal and building an API facade toodecouple from internal implementations that are not ripe for partner consumption.</span></span>
* <span data-ttu-id="c58bb-111">**Spuštění interního programu s rozhraní API** prostřednictvím nabídky centralizovaného umístění hello organizace toocommunicate o dostupnosti hello a nejnovější změny tooAPIs, prostřednictvím brány přístup na základě organizační účtů, všechny na základě zabezpečeného kanálu mezi Brána Hello rozhraní API a back-end hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-111">**Running an internal API program** by offering a centralized location for hello organization toocommunicate about hello availability and latest changes tooAPIs, gating access based on organizational accounts, all based on a secured channel between hello API gateway and hello backend.</span></span>

<span data-ttu-id="c58bb-112">Hello systému se skládá z hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="c58bb-112">hello system is made up of hello following components:</span></span>

* <span data-ttu-id="c58bb-113">Hello **Brána rozhraní API** je koncový bod hello který:</span><span class="sxs-lookup"><span data-stu-id="c58bb-113">hello **API gateway** is hello endpoint that:</span></span>
  
  * <span data-ttu-id="c58bb-114">Přijímá volání rozhraní API a směruje je back-EndY tooyour.</span><span class="sxs-lookup"><span data-stu-id="c58bb-114">Accepts API calls and routes them tooyour backends.</span></span>
  * <span data-ttu-id="c58bb-115">Ověřuje klíče rozhraní API, tokeny JWT, certifikáty a další přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c58bb-115">Verifies API keys, JWT tokens, certificates, and other credentials.</span></span>
  * <span data-ttu-id="c58bb-116">Vynucuje kvóty využití a omezení četnosti.</span><span class="sxs-lookup"><span data-stu-id="c58bb-116">Enforces usage quotas and rate limits.</span></span>
  * <span data-ttu-id="c58bb-117">Transformuje rozhraní API hello chodu beze změny kódu.</span><span class="sxs-lookup"><span data-stu-id="c58bb-117">Transforms your API on hello fly without code modifications.</span></span>
  * <span data-ttu-id="c58bb-118">Ukládá odezvy back-endu do určené mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="c58bb-118">Caches backend responses where set up.</span></span>
  * <span data-ttu-id="c58bb-119">Protokoluje metadata volání pro účely analýzy.</span><span class="sxs-lookup"><span data-stu-id="c58bb-119">Logs call metadata for analytics purposes.</span></span>
* <span data-ttu-id="c58bb-120">Hello **portál vydavatele** je rozhraní pro správu hello kterém můžete nastavit program s rozhraním API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-120">hello **publisher portal** is hello administrative interface where you set up your API program.</span></span> <span data-ttu-id="c58bb-121">Použijte ho k následujícím akcím:</span><span class="sxs-lookup"><span data-stu-id="c58bb-121">Use it to:</span></span>
  
  * <span data-ttu-id="c58bb-122">definování nebo import schématu rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c58bb-122">Define or import API schema.</span></span>
  * <span data-ttu-id="c58bb-123">balení rozhraní API do produktů</span><span class="sxs-lookup"><span data-stu-id="c58bb-123">Package APIs into products.</span></span>
  * <span data-ttu-id="c58bb-124">Nastavení zásad, například kvót nebo transformací hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-124">Set up policies like quotas or transformations on hello APIs.</span></span>
  * <span data-ttu-id="c58bb-125">získání přehledů z analýz</span><span class="sxs-lookup"><span data-stu-id="c58bb-125">Get insights from analytics.</span></span>
  * <span data-ttu-id="c58bb-126">správa uživatelů</span><span class="sxs-lookup"><span data-stu-id="c58bb-126">Manage users.</span></span>
* <span data-ttu-id="c58bb-127">Hello **portál pro vývojáře** funguje jako hlavní webová služba hello pro vývojáře, kde můžete:</span><span class="sxs-lookup"><span data-stu-id="c58bb-127">hello **developer portal** serves as hello main web presence for developers, where they can:</span></span>
  
  * <span data-ttu-id="c58bb-128">Číst dokumentaci k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-128">Read API documentation.</span></span>
  * <span data-ttu-id="c58bb-129">Vyzkoušejte rozhraní API prostřednictvím interaktivní konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-129">Try out an API via hello interactive console.</span></span>
  * <span data-ttu-id="c58bb-130">Vytvořit účet a přihlásit se klíče tooget rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-130">Create an account and subscribe tooget API keys.</span></span>
  * <span data-ttu-id="c58bb-131">Přistupovat k analýzám jejich využití.</span><span class="sxs-lookup"><span data-stu-id="c58bb-131">Access analytics on their own usage.</span></span>

## <span data-ttu-id="c58bb-132"><a name="create-service-instance"></a>Vytvoření instance API Managementu</span><span class="sxs-lookup"><span data-stu-id="c58bb-132"><a name="create-service-instance"> </a>Create an API Management instance</span></span>
> [!NOTE]
> <span data-ttu-id="c58bb-133">toocomplete tohoto kurzu potřebujete účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c58bb-133">toocomplete this tutorial, you need an Azure account.</span></span> <span data-ttu-id="c58bb-134">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="c58bb-134">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="c58bb-135">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][Azure Free Trial].</span><span class="sxs-lookup"><span data-stu-id="c58bb-135">For details, see [Azure Free Trial][Azure Free Trial].</span></span>
> 
> 

<span data-ttu-id="c58bb-136">Hello prvním krokem při práci se službou API Management je toocreate instance služby.</span><span class="sxs-lookup"><span data-stu-id="c58bb-136">hello first step in working with API Management is toocreate a service instance.</span></span> <span data-ttu-id="c58bb-137">Přihlaste se toohello [portálu Azure] [ Azure Portal] a klikněte na tlačítko **nový**, **Web + mobilní**, **API Management**.</span><span class="sxs-lookup"><span data-stu-id="c58bb-137">Sign in toohello [Azure Portal][Azure Portal] and click **New**, **Web + Mobile**, **API Management**.</span></span>

![Nová instance služby API Management][api-management-create-instance-menu]

<span data-ttu-id="c58bb-139">Pro **název**, zadejte adresu URL služby hello toouse název jedinečné subdomény.</span><span class="sxs-lookup"><span data-stu-id="c58bb-139">For **Name**, specify a unique sub-domain name toouse for hello service URL.</span></span>

<span data-ttu-id="c58bb-140">Zvolte hello potřeby **předplatné**, **skupiny prostředků** a **umístění** pro instanci služby.</span><span class="sxs-lookup"><span data-stu-id="c58bb-140">Choose hello desired **Subscription**, **Resource group** and **Location** for your service instance.</span></span>

<span data-ttu-id="c58bb-141">Zadejte **Contoso Ltd.** pro hello **název organizace**a zadejte e-mailovou adresu v hello **e-mailu správce** pole.</span><span class="sxs-lookup"><span data-stu-id="c58bb-141">Enter **Contoso Ltd.** for hello **Organization Name**, and enter your email address in hello **Administrator E-Mail** field.</span></span>

> [!NOTE]
> <span data-ttu-id="c58bb-142">Tato e-mailová adresa se používá pro oznámení z hello systému API Management.</span><span class="sxs-lookup"><span data-stu-id="c58bb-142">This email address is used for notifications from hello API Management system.</span></span> <span data-ttu-id="c58bb-143">Další informace najdete v tématu [jak tooconfigure oznámení a e-mailových šablon ve službě Azure API Management][How tooconfigure notifications and email templates in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="c58bb-143">For more information, see [How tooconfigure notifications and email templates in Azure API Management][How tooconfigure notifications and email templates in Azure API Management].</span></span>
> 
> 

![Nová služba API Management][api-management-create-instance-step1]

<span data-ttu-id="c58bb-145">Instance služby API Management jsou dostupné ve třech úrovních: Developer (vývojář), Standard a Premium.</span><span class="sxs-lookup"><span data-stu-id="c58bb-145">API Management service instances are available in three tiers: Developer, Standard, and Premium.</span></span>

> [!NOTE]
> <span data-ttu-id="c58bb-146">Úroveň Developer Hello je pro vývoj, testování a pilotní programy rozhraní API, kde vysokou dostupnost nehrají důležitou roli.</span><span class="sxs-lookup"><span data-stu-id="c58bb-146">hello Developer Tier is for development, testing, and pilot API programs where high availability is not a concern.</span></span> <span data-ttu-id="c58bb-147">V hello Standard a Premium vrstev je možné škálovat vaše toohandle počet jednotku rezervovanou větší provoz.</span><span class="sxs-lookup"><span data-stu-id="c58bb-147">In hello Standard and Premium tiers, you can scale your reserved unit count toohandle more traffic.</span></span> <span data-ttu-id="c58bb-148">úrovně Standard a Premium Hello poskytnout služby API Management s hello většina výpočetní výkon a zlepšit výkon.</span><span class="sxs-lookup"><span data-stu-id="c58bb-148">hello Standard and Premium tiers provide your API Management service with hello most processing power and performance.</span></span> <span data-ttu-id="c58bb-149">Tento kurz můžete dokončit pomocí libovolné úrovně.</span><span class="sxs-lookup"><span data-stu-id="c58bb-149">You can complete this tutorial by using any tier.</span></span> <span data-ttu-id="c58bb-150">Další informace o úrovních služby API Management najdete v článku [Ceny služby API Management][API Management pricing].</span><span class="sxs-lookup"><span data-stu-id="c58bb-150">For more information about API Management tiers, see [API Management pricing][API Management pricing].</span></span>
> 
> 

<span data-ttu-id="c58bb-151">Klikněte na tlačítko **vytvořit** toostart zřizování instanci služby.</span><span class="sxs-lookup"><span data-stu-id="c58bb-151">Click **Create** toostart provisioning your service instance.</span></span>

![Nová služba API Management][api-management-instance-created]

<span data-ttu-id="c58bb-153">Po vytvoření instance služby hello hello dalším krokem je toocreate nebo import rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-153">Once hello service instance is created, hello next step is toocreate or import an API.</span></span>

## <span data-ttu-id="c58bb-154"><a name="create-api"></a>Import rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c58bb-154"><a name="create-api"> </a>Import an API</span></span>
<span data-ttu-id="c58bb-155">Rozhraní API se skládá ze sady operací, které můžete vyvolat z klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="c58bb-155">An API consists of a set of operations that can be invoked from a client application.</span></span> <span data-ttu-id="c58bb-156">Operace rozhraní API jsou směrovány přes proxy server tooexisting webové služby.</span><span class="sxs-lookup"><span data-stu-id="c58bb-156">API operations are proxied tooexisting web services.</span></span>

<span data-ttu-id="c58bb-157">Rozhraní API můžete vytvořit (a operace přidat) ručně, nebo je můžete importovat.</span><span class="sxs-lookup"><span data-stu-id="c58bb-157">APIs can be created (and operations can be added) manually, or they can be imported.</span></span> <span data-ttu-id="c58bb-158">V tomto kurzu naimportujeme rozhraní API hello ukázka kalkulačku webové služby poskytované společností Microsoft a je hostovaná v Azure.</span><span class="sxs-lookup"><span data-stu-id="c58bb-158">In this tutorial, we will import hello API for a sample calculator web service provided by Microsoft and hosted on Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="c58bb-159">Pokyny týkající se vytváření rozhraní API a ručnímu přidávání operací najdete v tématu [jak toocreate rozhraní API](api-management-howto-create-apis.md) a [jak tooan tooadd operace rozhraní API](api-management-howto-add-operations.md).</span><span class="sxs-lookup"><span data-stu-id="c58bb-159">For guidance on creating an API and manually adding operations, see [How toocreate APIs](api-management-howto-create-apis.md) and [How tooadd operations tooan API](api-management-howto-add-operations.md).</span></span>
> 
> 

<span data-ttu-id="c58bb-160">Rozhraní API se konfigurují na portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-160">APIs are configured from hello publisher portal.</span></span> <span data-ttu-id="c58bb-161">tooreach, klikněte na tlačítko **portál vydavatele** z panelu nástrojů služby hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-161">tooreach it, click **Publisher portal** from hello service toolbar.</span></span>

![Portál vydavatele][api-management-management-console]

<span data-ttu-id="c58bb-163">tooimport hello kalkulačky rozhraní API, klikněte na tlačítko **rozhraní API** z hello **API Management** nabídky na levé hello a pak klikněte na tlačítko **rozhraní API pro Import**.</span><span class="sxs-lookup"><span data-stu-id="c58bb-163">tooimport hello calculator API, click **APIs** from hello **API Management** menu on hello left, and then click **Import API**.</span></span>

![Tlačítko Importovat rozhraní API][api-management-import-api]

<span data-ttu-id="c58bb-165">Proveďte následující kroky rozhraní API kalkulačky hello tooconfigure hello:</span><span class="sxs-lookup"><span data-stu-id="c58bb-165">Perform hello following steps tooconfigure hello calculator API:</span></span>

1. <span data-ttu-id="c58bb-166">Klikněte na tlačítko **z adresy URL**, zadejte **http://calcapi.cloudapp.net/calcapi.json** do hello **adresu URL dokumentu specifikace** textové pole a klikněte na tlačítko hello **Swagger**  přepínač.</span><span class="sxs-lookup"><span data-stu-id="c58bb-166">Click **From URL**, enter **http://calcapi.cloudapp.net/calcapi.json** into hello **Specification document URL** text box, and click hello **Swagger** radio button.</span></span>
2. <span data-ttu-id="c58bb-167">Typ **calc** do hello **přípona adresy URL webového rozhraní API** textové pole.</span><span class="sxs-lookup"><span data-stu-id="c58bb-167">Type **calc** into hello **Web API URL suffix** text box.</span></span>
3. <span data-ttu-id="c58bb-168">Klikněte na tlačítko v hello **produkty (volitelné)** pole a zvolte **Starter**.</span><span class="sxs-lookup"><span data-stu-id="c58bb-168">Click in hello **Products (optional)** box and choose **Starter**.</span></span>
4. <span data-ttu-id="c58bb-169">Klikněte na tlačítko **Uložit** tooimport hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-169">Click **Save** tooimport hello API.</span></span>

![Přidání nového rozhraní API][api-management-import-new-api]

> [!NOTE]
> <span data-ttu-id="c58bb-171">**API Management** aktuálně podporuje import dokumentu Swagger ve verzi 1.2 a 2.0.</span><span class="sxs-lookup"><span data-stu-id="c58bb-171">**API Management** currently supports both 1.2 and 2.0 version of Swagger document for import.</span></span> <span data-ttu-id="c58bb-172">I když [specifikace Swagger 2.0](http://swagger.io/specification) uvádí, že vlastnosti `host`, `basePath` a `schemes` jsou volitelné, váš dokument Swagger 2.0 **MUSÍ** tyto vlastnosti obsahovat, jinak nepůjde importovat.</span><span class="sxs-lookup"><span data-stu-id="c58bb-172">Make sure that, even though [Swagger 2.0 specification](http://swagger.io/specification) declares that `host`, `basePath`, and `schemes` properties are optional, your Swagger 2.0 document **MUST** contain those properties; otherwise it won't get imported.</span></span> 
> 
> 

<span data-ttu-id="c58bb-173">Po importu rozhraní API hello hello souhrnná stránka rozhraní API hello zobrazí na portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-173">Once hello API is imported, hello summary page for hello API is displayed in hello publisher portal.</span></span>

![Souhrn rozhraní API][api-management-imported-api-summary]

<span data-ttu-id="c58bb-175">Hello část rozhraní API obsahuje několik karet.</span><span class="sxs-lookup"><span data-stu-id="c58bb-175">hello API section has several tabs.</span></span> <span data-ttu-id="c58bb-176">Hello **Souhrn** karta zobrazuje základní metriky a informace o hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-176">hello **Summary** tab displays basic metrics and information about hello API.</span></span> <span data-ttu-id="c58bb-177">Hello [nastavení](api-management-howto-create-apis.md#configure-api-settings) karta je použité tooview a úpravy hello konfiguraci pro rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-177">hello [Settings](api-management-howto-create-apis.md#configure-api-settings) tab is used tooview and edit hello configuration for an API.</span></span> <span data-ttu-id="c58bb-178">Hello [Operations](api-management-howto-add-operations.md) karta je operace použité toomanage hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-178">hello [Operations](api-management-howto-add-operations.md) tab is used toomanage hello API's operations.</span></span> <span data-ttu-id="c58bb-179">Hello **zabezpečení** karta může být použité tooconfigure ověřování brány pro back-end server hello pomocí základního ověření nebo [vzájemného ověření certifikátů](api-management-howto-mutual-certificates.md)a tooconfigure [ autorizace uživatelů pomocí standardu OAuth 2.0](api-management-howto-oauth2.md).</span><span class="sxs-lookup"><span data-stu-id="c58bb-179">hello **Security** tab can be used tooconfigure gateway authentication for hello backend server by using Basic authentication or [mutual certificate authentication](api-management-howto-mutual-certificates.md), and tooconfigure [user authorization by using OAuth 2.0](api-management-howto-oauth2.md).</span></span>  <span data-ttu-id="c58bb-180">Hello **problémy** karta je použité tooview problémy hlášené hello vývojáře, kteří používají vaše rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-180">hello **Issues** tab is used tooview issues reported by hello developers who are using your APIs.</span></span> <span data-ttu-id="c58bb-181">Hello **produkty** karta je použité tooconfigure hello produkty, které toto rozhraní API obsahují.</span><span class="sxs-lookup"><span data-stu-id="c58bb-181">hello **Products** tab is used tooconfigure hello products that contain this API.</span></span>

<span data-ttu-id="c58bb-182">Ve výchozím nastavení každá instance služby API Management obsahuje dva ukázkové produkty:</span><span class="sxs-lookup"><span data-stu-id="c58bb-182">By default, each API Management instance comes with two sample products:</span></span>

* <span data-ttu-id="c58bb-183">**Starter**</span><span class="sxs-lookup"><span data-stu-id="c58bb-183">**Starter**</span></span>
* <span data-ttu-id="c58bb-184">**Unlimited**</span><span class="sxs-lookup"><span data-stu-id="c58bb-184">**Unlimited**</span></span>

<span data-ttu-id="c58bb-185">V tomto kurzu přidala hello rozhraní API základní kalkulačky produktu Starter toohello při importu hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-185">In this tutorial, hello Basic Calculator API was added toohello Starter product when hello API was imported.</span></span>

<span data-ttu-id="c58bb-186">V pořadí toomake volání tooan rozhraní API vývojáři musí nejdřív přihlásit k odběru tooa produkt, který jim poskytne přístup tooit.</span><span class="sxs-lookup"><span data-stu-id="c58bb-186">In order toomake calls tooan API, developers must first subscribe tooa product that gives them access tooit.</span></span> <span data-ttu-id="c58bb-187">Vývojáři se přihlásit k odběru tooproducts v portálu pro vývojáře hello nebo správci přihlásit k odběru tooproducts vývojáři portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-187">Developers can subscribe tooproducts in hello developer portal, or administrators can subscribe developers tooproducts in hello publisher portal.</span></span> <span data-ttu-id="c58bb-188">Vzhledem k tomu, že jste vytvořili instance služby API Management hello v hello předchozí kroky v kurzu hello, takže jste už odebírané tooevery produktu ve výchozím nastavení jste správcem.</span><span class="sxs-lookup"><span data-stu-id="c58bb-188">You are an administrator since you created hello API Management instance in hello previous steps in hello tutorial, so you are already subscribed tooevery product by default.</span></span>

## <span data-ttu-id="c58bb-189"><a name="call-operation"></a>Volání operace z portálu pro vývojáře hello</span><span class="sxs-lookup"><span data-stu-id="c58bb-189"><a name="call-operation"> </a>Call an operation from hello developer portal</span></span>
<span data-ttu-id="c58bb-190">Operace lze volat přímo z portálu pro vývojáře hello, který nabízí pohodlný způsob tooview a otestovat hello operace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c58bb-190">Operations can be called directly from hello developer portal, which provides a convenient way tooview and test hello operations of an API.</span></span> <span data-ttu-id="c58bb-191">V tomto kroku kurzu budete volat hello základní rozhraní API kalkulačky **přidat dvě celá čísla** operaci.</span><span class="sxs-lookup"><span data-stu-id="c58bb-191">In this tutorial step, you will call hello Basic Calculator API's **Add two integers** operation.</span></span> <span data-ttu-id="c58bb-192">Klikněte na tlačítko **portál pro vývojáře** hello nabídce v hello top pravé části portálu vydavatele hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-192">Click **Developer portal** from hello menu at hello top right of hello publisher portal.</span></span>

![Portál pro vývojáře][api-management-developer-portal-menu]

<span data-ttu-id="c58bb-194">Klikněte na tlačítko **rozhraní API** z hello horní nabídce a pak klikněte na tlačítko **Basic Calculator** toosee hello dostupné operace.</span><span class="sxs-lookup"><span data-stu-id="c58bb-194">Click **APIs** from hello top menu, and then click **Basic Calculator** toosee hello available operations.</span></span>

![Portál pro vývojáře][api-management-developer-portal-calc-api]

<span data-ttu-id="c58bb-196">Všimněte si hello ukázkových popisů a parametrů naimportovaných společně s hello rozhraní API a operacemi, které poskytují dokumentaci vývojářům hello, které budou používat tuto operaci.</span><span class="sxs-lookup"><span data-stu-id="c58bb-196">Note hello sample descriptions and parameters that were imported along with hello API and operations, providing documentation for hello developers that will use this operation.</span></span> <span data-ttu-id="c58bb-197">Tyto popisy můžete přidat i v případě, kdy operace přidáváte ručně.</span><span class="sxs-lookup"><span data-stu-id="c58bb-197">These descriptions can also be added when operations are added manually.</span></span>

<span data-ttu-id="c58bb-198">toocall hello **přidat dvě celá čísla** operace, klikněte na tlačítko **vyzkoušet**.</span><span class="sxs-lookup"><span data-stu-id="c58bb-198">toocall hello **Add two integers** operation, click **Try it**.</span></span>

![Vyzkoušet][api-management-developer-portal-calc-api-console]

<span data-ttu-id="c58bb-200">Zadejte hodnoty pro parametry hello nebo ponechat výchozí nastavení hello a pak klikněte na tlačítko **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="c58bb-200">You can enter some values for hello parameters or keep hello defaults, and then click **Send**.</span></span>

![Metoda HTTP Get][api-management-invoke-get]

<span data-ttu-id="c58bb-202">Po vyvolání operace portál pro vývojáře hello zobrazí hello **stav odpovědi**, hello **hlavičky odpovědi**a jakýkoli **obsah odpovědi**.</span><span class="sxs-lookup"><span data-stu-id="c58bb-202">After an operation is invoked, hello developer portal displays hello **Response status**, hello **Response headers**, and any **Response content**.</span></span>

![Odpověď][api-management-invoke-get-response]

## <span data-ttu-id="c58bb-204"><a name="view-analytics"></a>Zobrazení analýzy</span><span class="sxs-lookup"><span data-stu-id="c58bb-204"><a name="view-analytics"> </a>View analytics</span></span>
<span data-ttu-id="c58bb-205">tooview analýzu základní kalkulačky, portál vydavatele back toohello přepínač tak, že vyberete **spravovat** hello nabídce v hello top napravo od portál pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-205">tooview analytics for Basic Calculator, switch back toohello publisher portal by selecting **Manage** from hello menu at hello top right of hello developer portal.</span></span>

![Spravovat][api-management-manage-menu]

<span data-ttu-id="c58bb-207">Hello výchozím zobrazením portálu vydavatele hello je hello **řídicí panel**, který nabízí přehled o instanci služby API Management.</span><span class="sxs-lookup"><span data-stu-id="c58bb-207">hello default view for hello publisher portal is hello **Dashboard**, which provides an overview of your API Management instance.</span></span>

![Řídicí panel][api-management-dashboard]

<span data-ttu-id="c58bb-209">Hover hello myš graf hello pro **Basic Calculator** toosee hello konkrétní metriky využití hello hello rozhraní API pro dané časové období.</span><span class="sxs-lookup"><span data-stu-id="c58bb-209">Hover hello mouse over hello chart for **Basic Calculator** toosee hello specific metrics for hello usage of hello API for a given time period.</span></span>

> [!NOTE]
> <span data-ttu-id="c58bb-210">Pokud na grafu nevidíte žádné čáry, přepněte zpět toohello portál pro vývojáře a proveďte několik volání do hello rozhraní API, chvíli počkejte a pak se vraťte toohello řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="c58bb-210">If you don't see any lines on your chart, switch back toohello developer portal and make some calls into hello API, wait a few moments, and then come back toohello dashboard.</span></span>
> 
> 

<span data-ttu-id="c58bb-211">Klikněte na tlačítko **zobrazit podrobnosti** tooview hello souhrnná stránka hello API včetně větší verze hello zobrazí metriky.</span><span class="sxs-lookup"><span data-stu-id="c58bb-211">Click **View Details** tooview hello summary page for hello API, including a larger version of hello displayed metrics.</span></span>

![Analýza][api-management-mouse-over]

![Souhrn][api-management-api-summary-metrics]

<span data-ttu-id="c58bb-214">Podrobné metriky a sestavy, klikněte na tlačítko **Analytics** z hello **API Management** nabídky na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="c58bb-214">For detailed metrics and reports, click **Analytics** from hello **API Management** menu on hello left.</span></span>

![Přehled][api-management-analytics-overview]

<span data-ttu-id="c58bb-216">Hello **Analytics** obsahuje hello následující čtyři karty:</span><span class="sxs-lookup"><span data-stu-id="c58bb-216">hello **Analytics** section has hello following four tabs:</span></span>

* <span data-ttu-id="c58bb-217">**Na první pohled** poskytuje celkové využití a stavu metriky, stejně jako hello nejlepší vývojáře, nejlepší produkty, nejlepší rozhraní API a nejlepší operace.</span><span class="sxs-lookup"><span data-stu-id="c58bb-217">**At a glance** provides overall usage and health metrics, as well as hello top developers, top products, top APIs, and top operations.</span></span>
* <span data-ttu-id="c58bb-218">**Využití** poskytuje hlubší pohled na volání a šířku pásma rozhraní API, včetně zeměpisného rozlišení.</span><span class="sxs-lookup"><span data-stu-id="c58bb-218">**Usage** provides an in-depth look at API calls and bandwidth, including a geographical representation.</span></span>
* <span data-ttu-id="c58bb-219">**Stav** se zaměřuje na stavové kódy, úspěšnost mezipaměti, doby odezvy a doby odezvy rozhraní API a služby.</span><span class="sxs-lookup"><span data-stu-id="c58bb-219">**Health** focuses on status codes, cache success rates, response times, and API and service response times.</span></span>
* <span data-ttu-id="c58bb-220">**Aktivita** poskytuje sestavy, které podrobně uvádějí hello konkrétní aktivitu podle vývojáře, produktu, rozhraní API a operace.</span><span class="sxs-lookup"><span data-stu-id="c58bb-220">**Activity** provides reports that drill down on hello specific activity by developer, product, API, and operation.</span></span>

## <span data-ttu-id="c58bb-221"><a name="next-steps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="c58bb-221"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="c58bb-222">Zjistěte, jak příliš[ochrana rozhraní API omezením četnosti](api-management-howto-product-with-rules.md).</span><span class="sxs-lookup"><span data-stu-id="c58bb-222">Learn how too[Protect your API with rate limits](api-management-howto-product-with-rules.md).</span></span>

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add hello new API tooa product]: #add-api-to-product
[Subscribe toohello product that contains hello API]: #subscribe
[Call an operation from hello Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How toomanage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[How tooconfigure notifications and email templates in Azure API Management]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[API Management pricing]: http://azure.microsoft.com/pricing/details/api-management/

[Azure Portal]: https://portal.azure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
