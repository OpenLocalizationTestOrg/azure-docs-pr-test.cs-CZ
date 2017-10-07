---
title: "problémy Brána pro správu dat aaaTroubleshoot | Microsoft Docs"
description: "Poskytuje tipy tootroubleshoot problémy související tooData Brána pro správu."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 85dacc8a1e8d574d6e7d5b556c995cebdc148fde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a><span data-ttu-id="0b32e-103">Řešení potíží při použití Brány pro správu dat</span><span class="sxs-lookup"><span data-stu-id="0b32e-103">Troubleshoot issues with using Data Management Gateway</span></span>
<span data-ttu-id="0b32e-104">Tento článek obsahuje informace o řešení potíží s použitím brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="0b32e-104">This article provides information on troubleshooting issues with using Data Management Gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="0b32e-105">V tématu hello [Brána pro správu dat](data-factory-data-management-gateway.md) článku podrobné informace o bráně hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-105">See hello [Data Management Gateway](data-factory-data-management-gateway.md) article for detailed information about hello gateway.</span></span> <span data-ttu-id="0b32e-106">V tématu hello [přesun dat mezi místními a cloudovými](data-factory-move-data-between-onprem-and-cloud.md) článku návod přesouvání dat od tooMicrosoft databáze systému SQL Server místní úložiště objektů Blob v Azure pomocí hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-106">See hello [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for a walkthrough of moving data from an on-premises SQL Server database tooMicrosoft Azure Blob storage by using hello gateway.</span></span>
>
>

## <a name="failed-tooinstall-or-register-gateway"></a><span data-ttu-id="0b32e-107">Tooinstall nebo zaregistrovat bránu</span><span class="sxs-lookup"><span data-stu-id="0b32e-107">Failed tooinstall or register gateway</span></span>
### <a name="1-problem"></a><span data-ttu-id="0b32e-108">1. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-108">1. Problem</span></span>
<span data-ttu-id="0b32e-109">Zobrazí tato chybová zpráva při instalaci a registraci bránu, konkrétně při stahování hello brány instalační soubor.</span><span class="sxs-lookup"><span data-stu-id="0b32e-109">You see this error message when installing and registering a gateway, specifically, while downloading hello gateway installation file.</span></span>

`Unable tooconnect toohello remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a><span data-ttu-id="0b32e-110">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-110">Cause</span></span>
<span data-ttu-id="0b32e-111">Hello počítače, na který se pokoušíte tooinstall hello brány se nezdařilo toodownload hello nejnovější brány soubor instalace z centra Stažení hello kvůli tooa problém sítě.</span><span class="sxs-lookup"><span data-stu-id="0b32e-111">hello machine on which you are trying tooinstall hello gateway has failed toodownload hello latest gateway installation file from hello download center due tooa network issue.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-112">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-112">Resolution</span></span>
<span data-ttu-id="0b32e-113">Zkontrolujte toosee nastavení serveru proxy vaší brány firewall, zda text hello nastavení blokovat hello síťové připojení mezi počítači toohello hello [centra stahování softwaru společnosti](https://download.microsoft.com/)a aktualizovat nastavení hello odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0b32e-113">Check your firewall proxy server settings toosee whether hello settings block hello network connection from hello computer toohello [download center](https://download.microsoft.com/), and update hello settings accordingly.</span></span>

<span data-ttu-id="0b32e-114">Případně si můžete stáhnout instalační soubor hello hello nejnovější bránu z hello [centra stahování softwaru společnosti](https://www.microsoft.com/download/details.aspx?id=39717) na jiných počítačích, kteří mohou přistupovat k stažení softwaru společnosti Microsoft hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-114">Alternatively, you can download hello installation file for hello latest gateway from hello [download center](https://www.microsoft.com/download/details.aspx?id=39717) on other machines that can access hello download center.</span></span> <span data-ttu-id="0b32e-115">Můžete pak kopie hello instalační soubor toohello brány hostitelského počítače a potom ho spusťte ručně brána hello tooinstall a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="0b32e-115">You can then copy hello installer file toohello gateway host computer and run it manually tooinstall and update hello gateway.</span></span>

### <a name="2-problem"></a><span data-ttu-id="0b32e-116">2. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-116">2. Problem</span></span>
<span data-ttu-id="0b32e-117">Zobrazí tato chyba, pokud se pokoušíte se tooinstall bránu kliknutím **nainstalovat přímo na tomto počítači** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0b32e-117">You see this error when you're attempting tooinstall a gateway by clicking **install directly on this computer** in hello Azure portal.</span></span>

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a><span data-ttu-id="0b32e-118">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-118">Cause</span></span>
<span data-ttu-id="0b32e-119">Brána je už nainstalovaná na počítači hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-119">A gateway is already installed on hello machine.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-120">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-120">Resolution</span></span>
<span data-ttu-id="0b32e-121">Odinstalujte hello existující bránu na počítači hello a klikněte na tlačítko hello **nainstalovat přímo na tomto počítači** propojení znovu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-121">Uninstall hello existing gateway on hello machine and click hello **install directly on this computer** link again.</span></span>

### <a name="3-problem"></a><span data-ttu-id="0b32e-122">3. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-122">3. Problem</span></span>
<span data-ttu-id="0b32e-123">Tato chyba může zobrazit při registraci novou bránu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-123">You might see this error when registering a new gateway.</span></span>

`Error: hello gateway has encountered an error during registration.`

#### <a name="cause"></a><span data-ttu-id="0b32e-124">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-124">Cause</span></span>
<span data-ttu-id="0b32e-125">Může se tato zpráva pro jednu z následujících důvodů hello:</span><span class="sxs-lookup"><span data-stu-id="0b32e-125">You might see this message for one of hello following reasons:</span></span>

* <span data-ttu-id="0b32e-126">Formát Hello hello klíč brány je neplatný.</span><span class="sxs-lookup"><span data-stu-id="0b32e-126">hello format of hello gateway key is invalid.</span></span>
* <span data-ttu-id="0b32e-127">klíč brány Hello byla zrušena platnost.</span><span class="sxs-lookup"><span data-stu-id="0b32e-127">hello gateway key has been invalidated.</span></span>
* <span data-ttu-id="0b32e-128">klíč brány Hello má byl znovu vygenerovat z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-128">hello gateway key has been regenerated from hello portal.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="0b32e-129">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-129">Resolution</span></span>
<span data-ttu-id="0b32e-130">Ověřte, zda používáte hello správné brány klíč z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-130">Verify whether you are using hello right gateway key from hello portal.</span></span> <span data-ttu-id="0b32e-131">V případě potřeby znovu vygenerovat klíč a použít hello klíče tooregister hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-131">If needed, regenerate a key and use hello key tooregister hello gateway.</span></span>

### <a name="4-problem"></a><span data-ttu-id="0b32e-132">4. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-132">4. Problem</span></span>
<span data-ttu-id="0b32e-133">Může se zobrazit následující chybová zpráva, pokud se registrace brány hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-133">You might see hello following error message when you're registering a gateway.</span></span>

`Error: hello content or format of hello gateway key "{gatewayKey}" is invalid, please go tooazure portal toocreate one new gateway or regenerate hello gateway key.`



![Obsah nebo formát klíče je neplatný](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a><span data-ttu-id="0b32e-135">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-135">Cause</span></span>
<span data-ttu-id="0b32e-136">Hello obsah nebo formátu hello vstupní brána klíče je nesprávné.</span><span class="sxs-lookup"><span data-stu-id="0b32e-136">hello content or format of hello input gateway key is incorrect.</span></span> <span data-ttu-id="0b32e-137">Jedním z důvodů hello může být, že jste zkopírovali pouze část hello klíč z portálu hello nebo používáte neplatný klíč.</span><span class="sxs-lookup"><span data-stu-id="0b32e-137">One of hello reasons can be that you copied only a portion of hello key from hello portal or you're using an invalid key.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-138">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-138">Resolution</span></span>
<span data-ttu-id="0b32e-139">Generovat klíč brány hello portálu a použít hello kopie tlačítko toocopy hello celý klíč.</span><span class="sxs-lookup"><span data-stu-id="0b32e-139">Generate a gateway key in hello portal, and use hello copy button toocopy hello whole key.</span></span> <span data-ttu-id="0b32e-140">U této brány hello tooregister okno, vložte jej.</span><span class="sxs-lookup"><span data-stu-id="0b32e-140">Then paste it in this window tooregister hello gateway.</span></span>

### <a name="5-problem"></a><span data-ttu-id="0b32e-141">5. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-141">5. Problem</span></span>
<span data-ttu-id="0b32e-142">Může se zobrazit následující chybová zpráva, pokud se registrace brány hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-142">You might see hello following error message when you're registering a gateway.</span></span>

`Error: hello gateway key is invalid or empty. Specify a valid gateway key from hello portal.`

![Klíč brány je neplatný nebo prázdný](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a><span data-ttu-id="0b32e-144">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-144">Cause</span></span>
<span data-ttu-id="0b32e-145">byl znovu vygenerovat klíč brány Hello nebo hello brány je Odstraněná hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0b32e-145">hello gateway key has been regenerated or hello gateway has been deleted in hello Azure portal.</span></span> <span data-ttu-id="0b32e-146">Může také dojít, pokud instalační program hello Brána pro správu dat není nejnovější.</span><span class="sxs-lookup"><span data-stu-id="0b32e-146">It can also happen if hello Data Management Gateway setup is not latest.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-147">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-147">Resolution</span></span>
<span data-ttu-id="0b32e-148">Zkontrolujte, pokud instalační program hello Brána pro správu dat je hello nejnovější verzi, můžete najít nejnovější verzi hello na hello Microsoft [centra stahování softwaru společnosti](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span><span class="sxs-lookup"><span data-stu-id="0b32e-148">Check if hello Data Management Gateway setup is hello latest version, you can find hello latest version on hello Microsoft [download center](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span></span>

<span data-ttu-id="0b32e-149">Pokud instalační program je aktuální / nejnovější a brány stále existuje na portálu, generuje se nový klíč brány hello hello portál Azure a použít hello kopie tlačítko toocopy hello celý klíč a pak ji vložit u této brány hello tooregister okno.</span><span class="sxs-lookup"><span data-stu-id="0b32e-149">If setup is current/ latest and gateway still exists on Portal, regenerate hello gateway key in hello Azure portal, and use hello copy button toocopy hello whole key, and then paste it in this window tooregister hello gateway.</span></span> <span data-ttu-id="0b32e-150">Jinak znovu vytvořte hello brány a začněte znovu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-150">Otherwise, recreate hello gateway and start over.</span></span>

### <a name="6-problem"></a><span data-ttu-id="0b32e-151">6. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-151">6. Problem</span></span>
<span data-ttu-id="0b32e-152">Může se zobrazit následující chybová zpráva, pokud se registrace brány hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-152">You might see hello following error message when you're registering a gateway.</span></span>

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with hello status “Gateway key is invalid”`

![Klíč brány je neplatný nebo prázdný](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a><span data-ttu-id="0b32e-154">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-154">Cause</span></span>
<span data-ttu-id="0b32e-155">K této chybě může dojít, protože brána hello byl odstraněn nebo byla znovu vygenerovat klíč hello přidružené brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-155">This error might happen because either hello gateway has been deleted or hello associated gateway key has been regenerated.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-156">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-156">Resolution</span></span>
<span data-ttu-id="0b32e-157">Pokud byl odstraněn hello brány, znovu vytvořit bránu hello z hello portálu, klikněte na **zaregistrovat**, zkopírujte hello klíč z portálu hello, vložte ho a zkuste tooregister hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-157">If hello gateway has been deleted, re-create hello gateway from hello portal, click **Register**, copy hello key from hello portal, paste it, and try tooregister hello gateway.</span></span>

<span data-ttu-id="0b32e-158">Pokud brána hello stále existuje, ale byla znovu vygeneroval svůj klíč, použijte hello nového klíče tooregister hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-158">If hello gateway still exists but its key has been regenerated, use hello new key tooregister hello gateway.</span></span> <span data-ttu-id="0b32e-159">Pokud nemáte hello klíč, znovu vygenerujte klíč hello znovu z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-159">If you don’t have hello key, regenerate hello key again from hello portal.</span></span>

### <a name="7-problem"></a><span data-ttu-id="0b32e-160">7. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-160">7. Problem</span></span>
<span data-ttu-id="0b32e-161">Pokud se registrace brány, bude pravděpodobně třeba tooenter cestu a heslo pro certifikát.</span><span class="sxs-lookup"><span data-stu-id="0b32e-161">When you're registering a gateway, you might need tooenter path and password for a certificate.</span></span>

![Zadejte certifikát](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a><span data-ttu-id="0b32e-163">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-163">Cause</span></span>
<span data-ttu-id="0b32e-164">na jiných počítačích než byl zaregistrován Hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-164">hello gateway has been registered on other machines before.</span></span> <span data-ttu-id="0b32e-165">Během hello počáteční registrace brána byl přidružit k bráně hello šifrovací certifikát.</span><span class="sxs-lookup"><span data-stu-id="0b32e-165">During hello initial registration of a gateway, an encryption certificate has been associated with hello gateway.</span></span> <span data-ttu-id="0b32e-166">Hello certifikátu můžete buď samoobslužné generovat bránou hello ani poskytované hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="0b32e-166">hello certificate can either be self-generated by hello gateway or provided by hello user.</span></span>  <span data-ttu-id="0b32e-167">Tento certifikát je pověření použité tooencrypt hello data Store (propojené služby).</span><span class="sxs-lookup"><span data-stu-id="0b32e-167">This certificate is used tooencrypt credentials of hello data store (linked service).</span></span>  

![Export certifikátu](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

<span data-ttu-id="0b32e-169">Při obnovení hello brány na jiný hostitelský počítač, zobrazí Průvodce registrací hello dotaz pro tento certifikát toodecrypt pověření dříve šifrovaných s tímto certifikátem.</span><span class="sxs-lookup"><span data-stu-id="0b32e-169">When restoring hello gateway on a different host machine, hello registration wizard asks for this certificate toodecrypt credentials previously encrypted with this certificate.</span></span>  <span data-ttu-id="0b32e-170">Bez tohoto certifikátu hello přihlašovací údaje nelze dešifrovat hello nové brány a další kopie aktivity spuštěních přidružené k této nové brány se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="0b32e-170">Without this certificate, hello credentials cannot be decrypted by hello new gateway and subsequent copy activity executions associated with this new gateway will fail.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="0b32e-171">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-171">Resolution</span></span>
<span data-ttu-id="0b32e-172">Pokud certifikát přihlašovacích údajů hello byly exportovány z původní počítač brány hello pomocí hello **exportovat** na hello tlačítko **nastavení** kartě v správy Správce konfigurace brány dat, použijte hello Tady certifikát.</span><span class="sxs-lookup"><span data-stu-id="0b32e-172">If you have exported hello credential certificate from hello original gateway machine by using hello **Export** button on hello **Settings** tab in Data Management Gateway Configuration Manager, use hello certificate here.</span></span>

<span data-ttu-id="0b32e-173">Při obnovení bránu nedá Přeskočit tuto fázi.</span><span class="sxs-lookup"><span data-stu-id="0b32e-173">You cannot skip this stage when recovering a gateway.</span></span> <span data-ttu-id="0b32e-174">Pokud hello certifikát chybí, třeba toodelete hello brány z portálu hello a znovu vytvořit novou bránu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-174">If hello certificate is missing, you need toodelete hello gateway from hello portal and re-create a new gateway.</span></span>  <span data-ttu-id="0b32e-175">Kromě toho aktualizujte všechny propojené služby, které jsou brány související toohello nutnosti opětovného zadávání svých přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="0b32e-175">In addition, update all linked services that are related toohello gateway by reentering their credentials.</span></span>

### <a name="8-problem"></a><span data-ttu-id="0b32e-176">8. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-176">8. Problem</span></span>
<span data-ttu-id="0b32e-177">Může se zobrazit následující chybová zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-177">You might see hello following error message.</span></span>

`Error: hello remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a><span data-ttu-id="0b32e-178">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-178">Cause</span></span>
<span data-ttu-id="0b32e-179">Tato chyba se stane, když vaše brána je v prostředí, které vyžaduje HTTP proxy tooaccess prostředků z Internetu, nebo se změnilo heslo pro ověřování serveru serveru proxy, ale není příslušným způsobem aktualizuje v bránu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-179">This error happens when your gateway is in an environment that requires an HTTP proxy tooaccess Internet resources, or your proxy's authentication password is changed but it's not updated accordingly in your gateway.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-180">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-180">Resolution</span></span>
<span data-ttu-id="0b32e-181">Postupujte podle pokynů hello v hello [důležité informace o Proxy serveru](#proxy-server-considerations) části tohoto článku a konfigurace nastavení proxy serveru s Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0b32e-181">Follow hello instructions in hello [Proxy server considerations](#proxy-server-considerations) section of this article, and configure proxy settings with Data Management Gateway Configuration Manager.</span></span>

## <a name="gateway-is-online-with-limited-functionality"></a><span data-ttu-id="0b32e-182">Je brána online s omezenou funkčností</span><span class="sxs-lookup"><span data-stu-id="0b32e-182">Gateway is online with limited functionality</span></span>
### <a name="1-problem"></a><span data-ttu-id="0b32e-183">1. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-183">1. Problem</span></span>
<span data-ttu-id="0b32e-184">Zobrazí stav hello hello brány jako online s omezenou funkčností.</span><span class="sxs-lookup"><span data-stu-id="0b32e-184">You see hello status of hello gateway as online with limited functionality.</span></span>

#### <a name="cause"></a><span data-ttu-id="0b32e-185">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-185">Cause</span></span>
<span data-ttu-id="0b32e-186">Stav hello hello brány zobrazí jako online s omezenou funkčností pro jednu z následujících důvodů hello:</span><span class="sxs-lookup"><span data-stu-id="0b32e-186">You see hello status of hello gateway as online with limited functionality for one of hello following reasons:</span></span>

* <span data-ttu-id="0b32e-187">Brána se nemůže připojit toocloud služby přes Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0b32e-187">Gateway cannot connect toocloud service through Azure Service Bus.</span></span>
* <span data-ttu-id="0b32e-188">Cloudové služby se nemůže připojit toogateway přes službu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0b32e-188">Cloud service cannot connect toogateway through Service Bus.</span></span>

<span data-ttu-id="0b32e-189">Když je brána hello online s omezenou funkčností, nemusí být možné toouse hello Průvodce kopírováním služby Data Factory toocreate datových kanálů pro kopírování dat tooor z místní datová úložiště.</span><span class="sxs-lookup"><span data-stu-id="0b32e-189">When hello gateway is online with limited functionality, you might not be able toouse hello Data Factory Copy Wizard toocreate data pipelines for copying data tooor from on-premises data stores.</span></span> <span data-ttu-id="0b32e-190">Jako alternativní řešení můžete použít Editor služby Data Factory hello portálu, Visual Studio nebo Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0b32e-190">As a workaround, you can use Data Factory Editor in hello portal, Visual Studio, or Azure PowerShell.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-191">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-191">Resolution</span></span>
<span data-ttu-id="0b32e-192">Řešení tohoto problému (online s omezenou funkčností) je založen na tom, jestli nelze hello brány připojit toohello cloudové služby nebo hello jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="0b32e-192">Resolution for this issue (online with limited functionality) is based on whether hello gateway cannot connect toohello cloud service or hello other way.</span></span> <span data-ttu-id="0b32e-193">Hello následující oddíly poskytují tato řešení.</span><span class="sxs-lookup"><span data-stu-id="0b32e-193">hello following sections provide these resolutions.</span></span>

### <a name="2-problem"></a><span data-ttu-id="0b32e-194">2. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-194">2. Problem</span></span>
<span data-ttu-id="0b32e-195">Zobrazí následující chyba hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-195">You see hello following error.</span></span>

`Error: Gateway cannot connect toocloud service through service bus`

![Brána se nemůže připojit toocloud služby](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a><span data-ttu-id="0b32e-197">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-197">Cause</span></span>
<span data-ttu-id="0b32e-198">Brána se nemůže připojit toohello Cloudová služba přes službu Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0b32e-198">Gateway cannot connect toohello cloud service through Service Bus.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-199">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-199">Resolution</span></span>
<span data-ttu-id="0b32e-200">Postupujte podle těchto kroků tooget hello brány zpátky do online režimu:</span><span class="sxs-lookup"><span data-stu-id="0b32e-200">Follow these steps tooget hello gateway back online:</span></span>

1. <span data-ttu-id="0b32e-201">Povolí odchozí pravidla v počítači brány hello a podniková brána firewall hello IP adresu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-201">Allow IP address outbound rules on hello gateway machine and hello corporate firewall.</span></span> <span data-ttu-id="0b32e-202">Můžete najít IP adresy z protokolu událostí systému Windows hello (ID == 401): Pokus o byla provedena tooaccess soketu způsobem, jeho přístupovými oprávněními XX je zakázané. XX. XX. XX:9350.</span><span class="sxs-lookup"><span data-stu-id="0b32e-202">You can find IP addresses from hello Windows Event Log (ID == 401): An attempt was made tooaccess a socket in a way forbidden by its access permissions XX.XX.XX.XX:9350.</span></span>
* <span data-ttu-id="0b32e-203">Konfigurace nastavení proxy serveru na hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-203">Configure proxy settings on hello gateway.</span></span> <span data-ttu-id="0b32e-204">V tématu hello [důležité informace o Proxy serveru](#proxy-server-considerations) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0b32e-204">See hello [Proxy server considerations](#proxy-server-considerations) section for details.</span></span>
* <span data-ttu-id="0b32e-205">Povolte Odchozí porty v obou hello brány Windows Firewall v počítači brány hello a podniková brána firewall hello 9350-9354 a 5671.</span><span class="sxs-lookup"><span data-stu-id="0b32e-205">Enable outbound ports 5671 and 9350-9354 on both hello Windows Firewall on hello gateway machine and hello corporate firewall.</span></span> <span data-ttu-id="0b32e-206">V tématu hello [porty a brány firewall](#ports-and-firewall) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="0b32e-206">See hello [Ports and firewall](#ports-and-firewall) section for details.</span></span> <span data-ttu-id="0b32e-207">Tento krok je volitelný, ale doporučujeme ho důvodů výkonu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-207">This step is optional, but we recommend it for performance consideration.</span></span>

### <a name="3-problem"></a><span data-ttu-id="0b32e-208">3. Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-208">3. Problem</span></span>
<span data-ttu-id="0b32e-209">Zobrazí následující chyba hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-209">You see hello following error.</span></span>

`Error: Cloud service cannot connect toogateway through service bus.`

#### <a name="cause"></a><span data-ttu-id="0b32e-210">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-210">Cause</span></span>
<span data-ttu-id="0b32e-211">Přechodná chyba v připojení k síti.</span><span class="sxs-lookup"><span data-stu-id="0b32e-211">A transient error in network connectivity.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-212">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-212">Resolution</span></span>
<span data-ttu-id="0b32e-213">Postupujte podle těchto kroků tooget hello brány zpátky do online režimu:</span><span class="sxs-lookup"><span data-stu-id="0b32e-213">Follow these steps tooget hello gateway back online:</span></span>

1. <span data-ttu-id="0b32e-214">Počkejte několik minut, hello připojení se automaticky obnoví při hello chyba byla odstraněna.</span><span class="sxs-lookup"><span data-stu-id="0b32e-214">Wait for a couple of minutes, hello connectivity will be automatically recovered when hello error is gone.</span></span>
* <span data-ttu-id="0b32e-215">Pokud hello chyba přetrvává, restartujte službu Brána hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-215">If hello error persists, restart hello gateway service.</span></span>

## <a name="failed-tooauthor-linked-service"></a><span data-ttu-id="0b32e-216">Neúspěšné tooauthor propojené služby</span><span class="sxs-lookup"><span data-stu-id="0b32e-216">Failed tooauthor linked service</span></span>
### <a name="problem"></a><span data-ttu-id="0b32e-217">Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-217">Problem</span></span>
<span data-ttu-id="0b32e-218">Tato chyba se můžete setkat, když akci toouse správce přihlašovacích údajů hello portálu tooinput pověření pro nové propojené služby, nebo aktualizovat přihlašovací údaje pro existující propojenou službu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-218">You might see this error when you try toouse Credential Manager in hello portal tooinput credentials for a new linked service, or update credentials for an existing linked service.</span></span>

`Error: hello data store '<Server>/<Database>' cannot be reached. Check connection settings for hello data source.`

<span data-ttu-id="0b32e-219">Když se tato chyba, stránka nastavení hello nástroje Data Management Gateway Configuration Manager může vypadat například hello následující snímek obrazovky.</span><span class="sxs-lookup"><span data-stu-id="0b32e-219">When you see this error, hello settings page of Data Management Gateway Configuration Manager might look like hello following screenshot.</span></span>

![Databázi nelze získat přístup.](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a><span data-ttu-id="0b32e-221">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-221">Cause</span></span>
<span data-ttu-id="0b32e-222">certifikát SSL Hello může byla v počítači brány hello ztraceny.</span><span class="sxs-lookup"><span data-stu-id="0b32e-222">hello SSL certificate might have been lost on hello gateway machine.</span></span> <span data-ttu-id="0b32e-223">počítač brány Hello nelze načíst hello certifikát aktuálně používaný pro šifrování SSL.</span><span class="sxs-lookup"><span data-stu-id="0b32e-223">hello gateway computer cannot load hello certificate currently that is used for SSL encryption.</span></span> <span data-ttu-id="0b32e-224">Můžete si také prohlédnout chybové hlášení v protokolu událostí hello, který je podobný toohello následující zprávou.</span><span class="sxs-lookup"><span data-stu-id="0b32e-224">You might also see an error message in hello event log that is similar toohello following message.</span></span>

 `Unable tooget hello gateway settings from cloud service. Check hello gateway key and hello network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a><span data-ttu-id="0b32e-225">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-225">Resolution</span></span>
<span data-ttu-id="0b32e-226">Postupujte podle těchto kroků toosolve hello problému:</span><span class="sxs-lookup"><span data-stu-id="0b32e-226">Follow these steps toosolve hello problem:</span></span>

1. <span data-ttu-id="0b32e-227">Spusťte Správce konfigurace brány pro správu dat.</span><span class="sxs-lookup"><span data-stu-id="0b32e-227">Start Data Management Gateway Configuration Manager.</span></span>
2. <span data-ttu-id="0b32e-228">Přepínač toohello **nastavení** kartě.</span><span class="sxs-lookup"><span data-stu-id="0b32e-228">Switch toohello **Settings** tab.</span></span>  
3. <span data-ttu-id="0b32e-229">Klikněte na tlačítko hello **změnu** certifikát SSL hello toochange tlačítko.</span><span class="sxs-lookup"><span data-stu-id="0b32e-229">Click hello **Change** button toochange hello SSL certificate.</span></span>

   ![Tlačítko změnit certifikát](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. <span data-ttu-id="0b32e-231">Vyberte nový certifikát jako certifikát SSL hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-231">Select a new certificate as hello SSL certificate.</span></span> <span data-ttu-id="0b32e-232">Můžete použít libovolný certifikát SSL, který je generován můžete nebo některé z organizací.</span><span class="sxs-lookup"><span data-stu-id="0b32e-232">You can use any SSL certificate that is generated by you or any organization.</span></span>

   ![Zadejte certifikát](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a><span data-ttu-id="0b32e-234">Aktivita kopírování se nezdaří</span><span class="sxs-lookup"><span data-stu-id="0b32e-234">Copy activity fails</span></span>
### <a name="problem"></a><span data-ttu-id="0b32e-235">Problém</span><span class="sxs-lookup"><span data-stu-id="0b32e-235">Problem</span></span>
<span data-ttu-id="0b32e-236">Možná jste si všimli hello následující "UserErrorFailedToConnectToSqlserver" selhání po nastavení kanál hello portálu.</span><span class="sxs-lookup"><span data-stu-id="0b32e-236">You might notice hello following "UserErrorFailedToConnectToSqlserver" failure after you set up a pipeline in hello portal.</span></span>

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect tooSQL Server`

#### <a name="cause"></a><span data-ttu-id="0b32e-237">Příčina</span><span class="sxs-lookup"><span data-stu-id="0b32e-237">Cause</span></span>
<span data-ttu-id="0b32e-238">K tomu může dojít z různých důvodů a zmírnění se liší podle toho.</span><span class="sxs-lookup"><span data-stu-id="0b32e-238">This can happen for different reasons, and mitigation varies accordingly.</span></span>

#### <a name="resolution"></a><span data-ttu-id="0b32e-239">Řešení</span><span class="sxs-lookup"><span data-stu-id="0b32e-239">Resolution</span></span>
<span data-ttu-id="0b32e-240">Povolte odchozí připojení TCP přes port TCP 1433 nebo na hello na straně klienta Brána pro správu dat před připojením tooan SQL database.</span><span class="sxs-lookup"><span data-stu-id="0b32e-240">Allow outbound TCP connections over port TCP/1433 on hello Data Management Gateway client side before connecting tooan SQL database.</span></span>

<span data-ttu-id="0b32e-241">Pokud hello cílová databáze je databáze Azure SQL, zkontrolujte nastavení systému SQL Server brány firewall pro Azure také.</span><span class="sxs-lookup"><span data-stu-id="0b32e-241">If hello target database is an Azure SQL database, check SQL Server firewall settings for Azure as well.</span></span>

<span data-ttu-id="0b32e-242">Najdete v následující části tootest hello připojení toohello místní úložiště dat hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-242">See hello following section tootest hello connection toohello on-premises data store.</span></span>

## <a name="data-store-connection-or-driver-related-errors"></a><span data-ttu-id="0b32e-243">Připojení nebo chyby související s ovladačů úložiště dat</span><span class="sxs-lookup"><span data-stu-id="0b32e-243">Data store connection or driver-related errors</span></span>
<span data-ttu-id="0b32e-244">Pokud se zobrazí data uložit připojení nebo chyby související s ovladačem, proveďte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="0b32e-244">If you see data store connection or driver-related errors, complete hello following steps:</span></span>

1. <span data-ttu-id="0b32e-245">Spusťte Správce konfigurace brány pro správu dat na počítač brány hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-245">Start Data Management Gateway Configuration Manager on hello gateway machine.</span></span>
2. <span data-ttu-id="0b32e-246">Přepínač toohello **diagnostiky** kartě.</span><span class="sxs-lookup"><span data-stu-id="0b32e-246">Switch toohello **Diagnostics** tab.</span></span>
3. <span data-ttu-id="0b32e-247">V **Test připojení**, přidejte hodnoty pro skupinu hello brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-247">In **Test Connection**, add hello gateway group values.</span></span>
4. <span data-ttu-id="0b32e-248">Klikněte na tlačítko **Test** toosee, pokud připojíte toohello místní zdroj dat z počítače brány hello pomocí hello informace o připojení a přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0b32e-248">Click **Test** toosee if you can connect toohello on-premises data source from hello gateway machine by using hello connection information and credentials.</span></span> <span data-ttu-id="0b32e-249">Jestliže hello test připojení stále po instalaci ovladače, restartování hello brány toopick až hello nejnovější změny pro ni.</span><span class="sxs-lookup"><span data-stu-id="0b32e-249">If hello test connection still fails after you install a driver, restart hello gateway for it toopick up hello latest change.</span></span>

![Test připojení na kartě diagnostiky](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a><span data-ttu-id="0b32e-251">Protokoly Gateway</span><span class="sxs-lookup"><span data-stu-id="0b32e-251">Gateway logs</span></span>
### <a name="send-gateway-logs-toomicrosoft"></a><span data-ttu-id="0b32e-252">Odeslat protokoly tooMicrosoft brány</span><span class="sxs-lookup"><span data-stu-id="0b32e-252">Send gateway logs tooMicrosoft</span></span>
<span data-ttu-id="0b32e-253">Při kontaktování Microsoft Support tooget nápovědy vyřešit problémy brány, můžete být požádáni tooshare protokolů brány.</span><span class="sxs-lookup"><span data-stu-id="0b32e-253">When you contact Microsoft Support tooget help with troubleshooting gateway issues, you might be asked tooshare your gateway logs.</span></span> <span data-ttu-id="0b32e-254">Verze hello hello brány můžete sdílet protokoly požadované gateway s dvěma kliknutí na tlačítko v Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0b32e-254">With hello release of hello gateway, you can share required gateway logs with two button clicks in Data Management Gateway Configuration Manager.</span></span>    

1. <span data-ttu-id="0b32e-255">Přepínač toohello **diagnostiky** kartě v Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0b32e-255">Switch toohello **Diagnostics** tab in Data Management Gateway Configuration Manager.</span></span>

    ![Karta diagnostiku brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. <span data-ttu-id="0b32e-257">Klikněte na tlačítko **odeslat protokoly** toosee hello následující dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="0b32e-257">Click **Send Logs** toosee hello following dialog box.</span></span>

    ![Data Management Gateway odeslat protokoly](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. <span data-ttu-id="0b32e-259">(Volitelné) Klikněte na tlačítko **zobrazit protokoly** tooreview protokoly v prohlížeči událostí hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-259">(Optional) Click **view logs** tooreview logs in hello event viewer.</span></span>
4. <span data-ttu-id="0b32e-260">(Volitelné) Klikněte na tlačítko **o ochraně osobních údajů** tooreview Microsoft webové služby Zásady ochrany osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="0b32e-260">(Optional) Click **privacy** tooreview Microsoft web services privacy statement.</span></span>
5. <span data-ttu-id="0b32e-261">Jakmile budete spokojeni s jsou o tooupload, klikněte na tlačítko **odeslat protokoly** tooactually odeslat protokoly hello ze hello posledních sedmi dnech tooMicrosoft pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0b32e-261">When you are satisfied with what you are about tooupload, click **Send Logs** tooactually send hello logs from hello last seven days tooMicrosoft for troubleshooting.</span></span> <span data-ttu-id="0b32e-262">Měli byste vidět hello stav operace odesílání protokolů hello, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-262">You should see hello status of hello send-logs operation as shown in hello following screenshot.</span></span>

    ![Data Management Gateway odeslat protokoly stavu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. <span data-ttu-id="0b32e-264">Po dokončení operace hello zobrazí dialogové okno, jak ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-264">After hello operation is complete, you see a dialog box as shown in hello following screenshot.</span></span>

    ![Data Management Gateway odeslat protokoly stavu](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. <span data-ttu-id="0b32e-266">Uložit hello **ID sestavy** a sdílet je s Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="0b32e-266">Save hello **Report ID** and share it with Microsoft Support.</span></span> <span data-ttu-id="0b32e-267">ID sestavy Hello je použité toolocate hello brány protokoly, které jste nahráli k řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="0b32e-267">hello report ID is used toolocate hello gateway logs that you uploaded for troubleshooting.</span></span>  <span data-ttu-id="0b32e-268">ID sestavy Hello je také uloženy v prohlížeči událostí hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-268">hello report ID is also saved in hello event viewer.</span></span>  <span data-ttu-id="0b32e-269">Můžete najít prohlížením hello ID události "25" a zkontrolujte hello datum a čas.</span><span class="sxs-lookup"><span data-stu-id="0b32e-269">You can find it by looking at hello event ID “25”, and check hello date and time.</span></span>

    ![Data Management Gateway odeslat protokoly ID sestavy](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a><span data-ttu-id="0b32e-271">Archiv brány protokoly na hostitelském počítači brány</span><span class="sxs-lookup"><span data-stu-id="0b32e-271">Archive gateway logs on gateway host machine</span></span>
<span data-ttu-id="0b32e-272">Je několik scénářů, kdy máte problémy s brány a protokoly gateway nemohou sdílet přímo:</span><span class="sxs-lookup"><span data-stu-id="0b32e-272">There are some scenarios where you have gateway issues and you cannot share gateway logs directly:</span></span>

* <span data-ttu-id="0b32e-273">Ručně nainstalovat hello brány a zaregistrovat bránu hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-273">You manually install hello gateway and register hello gateway.</span></span>
* <span data-ttu-id="0b32e-274">Pokusíte tooregister hello brány pomocí obnoveného klíče v Data Management Gateway Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="0b32e-274">You try tooregister hello gateway with a regenerated key in Data Management Gateway Configuration Manager.</span></span>
* <span data-ttu-id="0b32e-275">Zkuste toosend protokoly a nemůže být připojen hostitelská služba brány pro hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-275">You try toosend logs and hello gateway host service cannot be connected.</span></span>

<span data-ttu-id="0b32e-276">Pro tyto scénáře můžete uložit protokoly gateway jako soubor zip a sdílet ho při kontaktujte podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0b32e-276">For these scenarios, you can save gateway logs as a zip file and share it when you contact Microsoft support.</span></span> <span data-ttu-id="0b32e-277">Například pokud obdržíte chybu při registraci brány hello jako ukazuje následující snímek obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-277">For example, if you receive an error while you register hello gateway as shown in hello following screenshot.</span></span>   

![Chyba registrace brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

<span data-ttu-id="0b32e-279">Klikněte na tlačítko hello **archivu protokoly gateway** propojit tooarchive a uložte protokoly a potom sdílet soubor zip hello s podporu společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0b32e-279">Click hello **Archive gateway logs** link tooarchive and save logs, and then share hello zip file with Microsoft support.</span></span>

![Protokoly archivu brány správy dat](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a><span data-ttu-id="0b32e-281">Najít protokoly gateway</span><span class="sxs-lookup"><span data-stu-id="0b32e-281">Locate gateway logs</span></span>
<span data-ttu-id="0b32e-282">Podrobné brány protokolu informace naleznete v protokolech událostí systému Windows hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-282">You can find detailed gateway log information in hello Windows event logs.</span></span>

1. <span data-ttu-id="0b32e-283">Spustit systém Windows **Prohlížeč událostí**.</span><span class="sxs-lookup"><span data-stu-id="0b32e-283">Start Windows **Event Viewer**.</span></span>
2. <span data-ttu-id="0b32e-284">Vyhledání protokolů v hello **protokoly aplikací a služeb** > **Brána pro správu dat** složky.</span><span class="sxs-lookup"><span data-stu-id="0b32e-284">Locate logs in hello **Application and Services Logs** > **Data Management Gateway** folder.</span></span>

 <span data-ttu-id="0b32e-285">Pokud se řešení potíží s problémy související s brány, vyhledejte úroveň události chyb v prohlížeči událostí hello.</span><span class="sxs-lookup"><span data-stu-id="0b32e-285">When you're troubleshooting gateway-related issues, look for error level events in hello event viewer.</span></span>

![Brána pro správu dat protokoly v prohlížeči událostí](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
