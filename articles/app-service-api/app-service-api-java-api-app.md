---
title: "aaaBuild a nasazení aplikace Java API v Azure App Service"
description: "Zjistěte, jak toocreate aplikace Java API balíček a jeho nasazení tooAzure služby App Service."
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 04/25/2017
ms.author: rachelap;robmcm
ms.openlocfilehash: a4056fec870b1c4bed8ee14bb0e748b3ee89b9e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a><span data-ttu-id="07e03-103">Sestavení a nasazení aplikace API Java v Azure App Service</span><span class="sxs-lookup"><span data-stu-id="07e03-103">Build and deploy a Java API app in Azure App Service</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="07e03-104">Tento kurz ukazuje, jak toocreate aplikaci Java a nasaďte ho tooAzure App Service API Apps pomocí [Git].</span><span class="sxs-lookup"><span data-stu-id="07e03-104">This tutorial shows how toocreate a Java application and deploy it tooAzure App Service API Apps using [Git].</span></span> <span data-ttu-id="07e03-105">Hello pokyny v tomto kurzu platí pro všechny operační systémy, které podporují Javu.</span><span class="sxs-lookup"><span data-stu-id="07e03-105">hello instructions in this tutorial can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="07e03-106">Hello kód v tomto kurzu je sestaven pomocí [Maven].</span><span class="sxs-lookup"><span data-stu-id="07e03-106">hello code in this tutorial is built using [Maven].</span></span> <span data-ttu-id="07e03-107">[Jax-RS] je použité toocreate hello služba RESTful a vygeneruje se podle hello [Swagger] metadat specifikace pomocí hello [editoru Swagger].</span><span class="sxs-lookup"><span data-stu-id="07e03-107">[Jax-RS] is used toocreate hello RESTful Service, and is generated based on hello [Swagger] metadata specification using hello [Swagger Editor].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07e03-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07e03-108">Prerequisites</span></span>
1. <span data-ttu-id="07e03-109">[Sada pro vývojáře Java 8]\(nebo novější)</span><span class="sxs-lookup"><span data-stu-id="07e03-109">[Java Developer's Kit 8] \(or later)</span></span>
2. <span data-ttu-id="07e03-110">Nástroj [Maven] nainstalovaný na počítači pro vývoj</span><span class="sxs-lookup"><span data-stu-id="07e03-110">[Maven] installed on your development machine</span></span>
3. <span data-ttu-id="07e03-111">Nástroj [Git] nainstalovaný na počítači pro vývoj</span><span class="sxs-lookup"><span data-stu-id="07e03-111">[Git] installed on your development machine</span></span>
4. <span data-ttu-id="07e03-112">Placená nebo [bezplatnou zkušební verzi] předplatné příliš[Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="07e03-112">A paid or [free trial] subscription too[Microsoft Azure]</span></span>
5. <span data-ttu-id="07e03-113">Testovací aplikace HTTP, například [Postman]</span><span class="sxs-lookup"><span data-stu-id="07e03-113">An HTTP test application like [Postman]</span></span>

## <a name="scaffold-hello-api-using-swaggerio"></a><span data-ttu-id="07e03-114">Rozhraní API hello vygenerované uživatelské rozhraní pomocí editoru Swagger.IO</span><span class="sxs-lookup"><span data-stu-id="07e03-114">Scaffold hello API using Swagger.IO</span></span>
<span data-ttu-id="07e03-115">Pomocí online editoru swagger.io hello, můžete zadat kód YAML nebo JSON pro Swagger představující strukturu hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="07e03-115">Using hello swagger.io online editor, you can enter Swagger JSON or YAML code representing hello structure of your API.</span></span> <span data-ttu-id="07e03-116">Jakmile máte hello API útoku na určený, můžete exportovat kódu pro různé platformy a rozhraní.</span><span class="sxs-lookup"><span data-stu-id="07e03-116">Once you have hello API surface area designed, you can export code for a variety of platforms and frameworks.</span></span> <span data-ttu-id="07e03-117">V další části hello hello vygeneroval kód bude upravené tooinclude imitované funkce.</span><span class="sxs-lookup"><span data-stu-id="07e03-117">In hello next section, hello scaffolded code will be modified tooinclude mock functionality.</span></span> 

<span data-ttu-id="07e03-118">Tato ukázka začíná s textu JSON pro Swagger, které vložíte do editoru swagger.io hello, který bude potom být použité toogenerate kód využitím JAX-RS tooaccess koncový bod REST API.</span><span class="sxs-lookup"><span data-stu-id="07e03-118">This demonstration will begin with a Swagger JSON body that you will paste into hello swagger.io editor, which will then be used toogenerate code making use of JAX-RS tooaccess a REST API endpoint.</span></span> <span data-ttu-id="07e03-119">Potom budete upravovat hello vygeneroval kód tooreturn imitovaná data, simulaci REST API vytvořené na mechanismu trvalosti.</span><span class="sxs-lookup"><span data-stu-id="07e03-119">Then, you'll edit hello scaffolded code tooreturn mock data, simulating a REST API built atop a data persistence mechanism.</span></span>  

1. <span data-ttu-id="07e03-120">Zkopírujte následující schránky tooyour kód JSON pro Swagger hello:</span><span class="sxs-lookup"><span data-stu-id="07e03-120">Copy hello following Swagger JSON code tooyour clipboard:</span></span>
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. <span data-ttu-id="07e03-121">Přejděte toohello [Online Swagger Editor].</span><span class="sxs-lookup"><span data-stu-id="07e03-121">Navigate toohello [Online Swagger Editor].</span></span> <span data-ttu-id="07e03-122">Potom klikněte hello **souboru -> Paste JSON** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="07e03-122">Once there, click hello **File -> Paste JSON** menu item.</span></span>
   
    ![Položka nabídky Paste JSON (Vložit JSON)][paste-json]
3. <span data-ttu-id="07e03-124">Vložte hello kontakty seznamu rozhraní API JSON pro Swagger jste zkopírovali dříve.</span><span class="sxs-lookup"><span data-stu-id="07e03-124">Paste in hello Contacts List API Swagger JSON you copied earlier.</span></span> 
   
    ![Vložení kódu JSON do Swaggeru][pasted-swagger]
4. <span data-ttu-id="07e03-126">Zobrazit hello stránky dokumentace a souhrn rozhraní API vykreslený v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="07e03-126">View hello documentation pages and API summary rendered in hello editor.</span></span> 
   
    ![Zobrazení dokumentů generovaných ve Swaggeru][view-swagger-generated-docs]
5. <span data-ttu-id="07e03-128">Vyberte hello **generovat Server -> JAX-RS** nabídky možnost tooscaffold hello serverový kód budete upravovat novější tooadd imitované implementace.</span><span class="sxs-lookup"><span data-stu-id="07e03-128">Select hello **Generate Server -> JAX-RS** menu option tooscaffold hello server-side code you'll edit later tooadd mock implementation.</span></span> 
   
    ![Generování položky nabídky kódu][generate-code-menu-item]
   
    <span data-ttu-id="07e03-130">Po vygenerování kódu hello budete je třeba zadat toodownload souboru ZIP.</span><span class="sxs-lookup"><span data-stu-id="07e03-130">Once hello code is generated, you'll be provided a ZIP file toodownload.</span></span> <span data-ttu-id="07e03-131">Tento soubor obsahuje hello kód automaticky vytvořený generátorem kódu Swagger hello a všechny přidružené skripty sestavení.</span><span class="sxs-lookup"><span data-stu-id="07e03-131">This file contains hello code scaffolded by hello Swagger code generator and all associated build scripts.</span></span> <span data-ttu-id="07e03-132">Rozbalte hello celou knihovnu tooa adresáře na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="07e03-132">Unzip hello entire library tooa directory on your development workstation.</span></span> 

## <a name="edit-hello-code-tooadd-api-implementation"></a><span data-ttu-id="07e03-133">Upravit kód tooadd hello implementace rozhraní API</span><span class="sxs-lookup"><span data-stu-id="07e03-133">Edit hello Code tooadd API Implementation</span></span>
<span data-ttu-id="07e03-134">V této části nahradíte hello generované Swagger kódu na straně serveru implementace vlastním kódem.</span><span class="sxs-lookup"><span data-stu-id="07e03-134">In this section, you'll replace hello Swagger-generated code's server-side implementation with your custom code.</span></span> <span data-ttu-id="07e03-135">Hello nový kód vrátí toohello entity kontaktů ArrayList pro volajícího klienta.</span><span class="sxs-lookup"><span data-stu-id="07e03-135">hello new code will return an ArrayList of Contact entities toohello calling client.</span></span> 

1. <span data-ttu-id="07e03-136">Otevřete hello *Contact.java* souboru modelu, který se nachází v hello *src/gen/java/vstupně-výstupní operace/swagger/model* složky, pomocí [Visual Studio Code] nebo svém oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="07e03-136">Open hello *Contact.java* model file, which is located in hello *src/gen/java/io/swagger/model* folder, using [Visual Studio Code] or your favorite text editor.</span></span> 
   
    ![Otevření souboru modelu kontaktů][open-contact-model-file]
2. <span data-ttu-id="07e03-138">Přidejte následující konstruktor v rámci hello hello **kontaktujte** třídy.</span><span class="sxs-lookup"><span data-stu-id="07e03-138">Add hello following constructor within hello **Contact** class.</span></span> 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. <span data-ttu-id="07e03-139">Otevřete hello *ContactsApiServiceImpl.java* soubor implementace služby, který je umístěný ve hello *src/main/java/vstupně-výstupní operace/swagger/rozhraní api/impl* složky, pomocí [Visual Studio Code]nebo svém oblíbeném textovém editoru.</span><span class="sxs-lookup"><span data-stu-id="07e03-139">Open hello *ContactsApiServiceImpl.java* service implementation file, which is located in hello *src/main/java/io/swagger/api/impl* folder, using [Visual Studio Code] or your favorite text editor.</span></span>
   
    ![Otevření souboru kódu služby kontaktů][open-contact-service-code-file]
4. <span data-ttu-id="07e03-141">Přepište hello kód v souboru hello tento nový kód tooadd kód služby toohello imitované implementace.</span><span class="sxs-lookup"><span data-stu-id="07e03-141">Overwrite hello code in hello file with this new code tooadd a mock implementation toohello service code.</span></span> 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. <span data-ttu-id="07e03-142">Otevřete příkazový řádek a změňte adresář toohello kořenové složky vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="07e03-142">Open a command prompt and change directory toohello root folder of your application.</span></span>
6. <span data-ttu-id="07e03-143">Spusťte následující kód hello toobuild příkazu Maven hello a spustit místně pomocí aplikačního serveru Jetty hello.</span><span class="sxs-lookup"><span data-stu-id="07e03-143">Execute hello following Maven command toobuild hello code and run it using hello Jetty app server locally.</span></span> 
   
        mvn package jetty:run
7. <span data-ttu-id="07e03-144">Měli byste vidět hello příkazové okno, že Jetty tento kód spustil na portu 8080.</span><span class="sxs-lookup"><span data-stu-id="07e03-144">You should see hello command window reflect that Jetty has started your code on port 8080.</span></span> 
   
    ![Otevření souboru kódu služby kontaktů][run-jetty-war]
8. <span data-ttu-id="07e03-146">Použití [Postman] metoda toomake žádost toohello "get all contacts" API na adrese http://localhost: 8080/api/contacts.</span><span class="sxs-lookup"><span data-stu-id="07e03-146">Use [Postman] toomake a request toohello "get all contacts" API method at http://localhost:8080/api/contacts.</span></span>
   
    ![Hello volání rozhraní API kontaktů][calling-contacts-api]
9. <span data-ttu-id="07e03-148">Použití [Postman] toomake žádost toohello "get specific contact" API metoda umístěné na adrese http://localhost: 8080/api/contacts/2.</span><span class="sxs-lookup"><span data-stu-id="07e03-148">Use [Postman] toomake a request toohello "get specific contact" API method located at http://localhost:8080/api/contacts/2.</span></span>
   
    ![Hello volání rozhraní API kontaktů][calling-specific-contact-api]
10. <span data-ttu-id="07e03-150">Nakonec sestavte soubor Java WAR (webový archiv) hello tak, že spustíte následující příkaz Mavenu v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="07e03-150">Finally, build hello Java WAR (Web ARchive) file by executing hello following Maven command in your console.</span></span> 
    
         mvn package war:war
11. <span data-ttu-id="07e03-151">Jakmile soubor WAR hello se umístí do hello **cíl** složky.</span><span class="sxs-lookup"><span data-stu-id="07e03-151">Once hello WAR file is built, it will be placed into hello **target** folder.</span></span> <span data-ttu-id="07e03-152">Přejděte do hello **cíl** složku a přejmenujte hello soubor WAR příliš**ROOT.war**.</span><span class="sxs-lookup"><span data-stu-id="07e03-152">Navigate into hello **target** folder and rename hello WAR file too**ROOT.war**.</span></span> <span data-ttu-id="07e03-153">(Ujistěte se, že hello použití velkých písmen odpovídá tomuto formátu).</span><span class="sxs-lookup"><span data-stu-id="07e03-153">(Make sure hello capitalization matches this format).</span></span>
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. <span data-ttu-id="07e03-154">Nakonec spuštěním následujících příkazů z kořenové složky vaší aplikace toocreate hello hello **nasazení** složky toouse toodeploy hello WAR souboru tooAzure.</span><span class="sxs-lookup"><span data-stu-id="07e03-154">Finally, execute hello following commands from hello root folder of your application toocreate a **deploy** folder toouse toodeploy hello WAR file tooAzure.</span></span> 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-hello-output-tooazure-app-service"></a><span data-ttu-id="07e03-155">Publikování tooAzure výstup hello služby App Service</span><span class="sxs-lookup"><span data-stu-id="07e03-155">Publish hello output tooAzure App Service</span></span>
<span data-ttu-id="07e03-156">V této části se dozvíte, jak toocreate nové aplikace API hello pomocí portálu Azure, tuto aplikaci API připravit na hostování aplikací Java a nasazení hello nově vytvořené WAR souboru tooAzure služby App Service toorun novou aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="07e03-156">In this section you'll learn how toocreate a new API App using hello Azure Portal, prepare that API App for hosting Java applications, and deploy hello newly-created WAR file tooAzure App Service toorun your new API App.</span></span> 

1. <span data-ttu-id="07e03-157">Vytvoření nové aplikace API v hello [portál Azure], kliknutím hello **nový -> Web + mobilní zařízení -> aplikace API** položky nabídky, zadáním podrobností o aplikaci a potom kliknutím na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="07e03-157">Create a new API app in hello [Azure portal], by clicking hello **New -> Web + Mobile -> API app** menu item, entering your app details, and then clicking **Create**.</span></span>
   
    ![Vytvoření nové aplikace API][create-api-app]
2. <span data-ttu-id="07e03-159">Po vytvoření aplikace API otevřete v této aplikaci **nastavení** okno a potom klikněte na hello **nastavení aplikace** položku nabídky.</span><span class="sxs-lookup"><span data-stu-id="07e03-159">Once your API app has been created, open your app's **Settings** blade, and then click hello **Application settings** menu item.</span></span> <span data-ttu-id="07e03-160">Vyberte hello nejnovější verzi Javy z dostupných možností hello, pak vyberte hello nejnovější verzi serveru Tomcat hello **webový kontejner** nabídce a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="07e03-160">Select hello latest Java versions from hello available options, then select hello latest Tomcat from hello **Web container** menu, and then click **Save**.</span></span>
   
    ![Nastavení Javy v okně aplikace API hello][set-up-java]
3. <span data-ttu-id="07e03-162">Klikněte na tlačítko hello **přihlašovací údaje nasazení** nabídky nastavení položky a zadejte uživatelské jméno a heslo, které chcete toouse pro publikování tooyour soubory aplikace API.</span><span class="sxs-lookup"><span data-stu-id="07e03-162">Click hello **Deployment credentials** settings menu item, and provide a username and password you wish toouse for publishing files tooyour API App.</span></span> 
   
    ![Nastavení přihlašovacích údajů nasazení][deployment-credentials]
4. <span data-ttu-id="07e03-164">Klikněte na tlačítko hello **zdroj nasazení** položku nabídky nastavení.</span><span class="sxs-lookup"><span data-stu-id="07e03-164">Click hello **Deployment source** settings menu item.</span></span> <span data-ttu-id="07e03-165">Potom klikněte hello **vybrat zdroj** tlačítko, vyberte hello **místní úložiště Git** a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="07e03-165">Once there, click hello **Choose source** button, select hello **Local Git Repository** option, and then click **OK**.</span></span> <span data-ttu-id="07e03-166">Tím se vytvoří úložiště Git spuštěné v Azure, které bude přidružené k vaší aplikaci API.</span><span class="sxs-lookup"><span data-stu-id="07e03-166">This will create a Git repository running in Azure, that has an association with your API App.</span></span> <span data-ttu-id="07e03-167">Pokaždé, když potvrdíte nějaký kód toohello *hlavní* větve úložiště Git, váš kód publikuje v živé, běžící instanci aplikace API.</span><span class="sxs-lookup"><span data-stu-id="07e03-167">Each time you commit code toohello *master* branch of your Git repository, your code will be published into your live running API App instance.</span></span> 
   
    ![Nastavení nového místního úložiště Git][select-git-repo]
5. <span data-ttu-id="07e03-169">Zkopírujte schránky tooyour URL hello nového úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="07e03-169">Copy hello new Git repository's URL tooyour clipboard.</span></span> <span data-ttu-id="07e03-170">Uložte ji, protože ji budete zanedlouho potřebovat.</span><span class="sxs-lookup"><span data-stu-id="07e03-170">Save this as it will be important in a moment.</span></span> 
   
    ![Nastavení nového úložiště Git pro vaši aplikaci][copy-git-repo-url]
6. <span data-ttu-id="07e03-172">Git push hello WAR toohello online úložiště souborů.</span><span class="sxs-lookup"><span data-stu-id="07e03-172">Git push hello WAR file toohello online repository.</span></span> <span data-ttu-id="07e03-173">toodo to, že přejdete do hello **nasazení** dříve vytvořená složka tak, aby mohli snadno Potvrdit kód hello až toohello úložiště spuštěného v App Service.</span><span class="sxs-lookup"><span data-stu-id="07e03-173">toodo this, navigate into hello **deploy** folder you created earlier so that you can easily commit hello code up toohello repository running in your App Service.</span></span> <span data-ttu-id="07e03-174">Jednou jste v okně konzoly hello a do hello složku, kde je umístěna, složka webapps hello vydávat hello postupu hello toolaunch příkazy Git a spusťte nasazení provedením.</span><span class="sxs-lookup"><span data-stu-id="07e03-174">Once you're in hello console window and navigated into hello folder where hello webapps folder is located, issue hello following Git commands toolaunch hello process and fire off a deployment.</span></span> 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    <span data-ttu-id="07e03-175">Jakmile odešlete hello **nabízené** požadavek, zobrazí se výzva k hello hesla, které jste předtím vytvořili pro přihlašovací údaje nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="07e03-175">Once you issue hello **push** request, you'll be asked for hello password you created for hello deployment credential earlier.</span></span> <span data-ttu-id="07e03-176">Po zadání přihlašovacích údajů, měli byste vidět, že vaše portálu zobrazení, které hello aktualizace byl nasazen.</span><span class="sxs-lookup"><span data-stu-id="07e03-176">After you enter your credentials, you should see your portal display that hello update was deployed.</span></span>
7. <span data-ttu-id="07e03-177">Pokud chcete znovu použít Postman toohit hello nově nasazené aplikace API spuštěné v Azure App Service, se zobrazí, zda text hello chování je konzistentní a že vrací kontaktní údaje podle očekávání, a pomocí jednoduchého kódu změny toohello Swagger.io automaticky generovaný kód Java.</span><span class="sxs-lookup"><span data-stu-id="07e03-177">If you once again use Postman toohit hello newly-deployed API App running in Azure App Service, you'll see that hello behavior is consistent and that now it is returning contact data as expected, and using simple code changes toohello Swagger.io scaffolded Java code.</span></span> 
   
    ![Použití živého REST API kontaktů Java v Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a><span data-ttu-id="07e03-179">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07e03-179">Next steps</span></span>
<span data-ttu-id="07e03-180">V tomto článku jste možnost toostart s soubor JSON pro Swagger a některé automaticky generovaný kód Java získaný v editoru Swagger.io hello.</span><span class="sxs-lookup"><span data-stu-id="07e03-180">In this article, you were able toostart with a Swagger JSON file and some scaffolded Java code obtained from hello Swagger.io editor.</span></span> <span data-ttu-id="07e03-181">Jednoduchými změnami kódu a procesem nasazení v Gitu jste získali funkční aplikaci API napsanou v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="07e03-181">From there, your simple changes and a Git deploy process resulted in having a functional API app written in Java.</span></span> <span data-ttu-id="07e03-182">Hello dalším kurzu se dozvíte, jak příliš[využívat aplikace API z klientů JavaScript pomocí CORS][App Service API CORS].</span><span class="sxs-lookup"><span data-stu-id="07e03-182">hello next tutorial shows how too[consume API apps from JavaScript clients, using CORS][App Service API CORS].</span></span> <span data-ttu-id="07e03-183">Další kurzy v hello řady zobrazit jak tooimplement ověřování a autorizace.</span><span class="sxs-lookup"><span data-stu-id="07e03-183">Later tutorials in hello series show how tooimplement authentication and authorization.</span></span>

<span data-ttu-id="07e03-184">toobuild s tímto příkladem, další informace o hello [sada SDK úložiště pro jazyk Java] toopersist hello JSON BLOB.</span><span class="sxs-lookup"><span data-stu-id="07e03-184">toobuild on this sample, you can learn more about hello [Storage SDK for Java] toopersist hello JSON blobs.</span></span> <span data-ttu-id="07e03-185">Nebo můžete použít hello [sada SDK Document DB Java] toosave vaše obraťte se na data tooAzure DB dokumentu.</span><span class="sxs-lookup"><span data-stu-id="07e03-185">Or, you could use hello [Document DB Java SDK] toosave your Contact data tooAzure Document DB.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="07e03-186">Viz také</span><span class="sxs-lookup"><span data-stu-id="07e03-186">See Also</span></span>
<span data-ttu-id="07e03-187">Další informace o používání Javy v Azure najdete na webu [Azure pro vývojáře v Javě](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="07e03-187">For more information about using Azure with Java, visit [Azure for Java developers](/java/azure).</span></span>

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[portál Azure]: https://portal.azure.com/
[sada SDK Document DB Java]: ../documentdb/documentdb-java-application.md
[bezplatnou zkušební verzi]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Sada pro vývojáře Java 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor2.swagger.io/
[Postman]: https://www.getpostman.com/
[sada SDK úložiště pro jazyk Java]:../storage/blobs/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[editoru Swagger]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
