---
title: "aaaView vrátil SAML hello služby Řízení přístupu (Java)"
description: "Zjistěte, jak tooview SAML vrácený hello služby Řízení přístupu v aplikacích Java hostované v Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: b6733bc98b505cfa89a4ce456f368ee15da11427
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-saml-returned-by-hello-azure-access-control-service"></a><span data-ttu-id="34a51-103">Jak tooview SAML vrácený hello služby Řízení přístupu Azure</span><span class="sxs-lookup"><span data-stu-id="34a51-103">How tooview SAML returned by hello Azure Access Control Service</span></span>
<span data-ttu-id="34a51-104">Tento průvodce vám ukáže, jak tooview hello základní Security (Assertion Markup Language SAML) vrátila tooyour aplikace hello služby Řízení přístupu Azure (ACS).</span><span class="sxs-lookup"><span data-stu-id="34a51-104">This guide will show you how tooview hello underlying Security Assertion Markup Language (SAML) returned tooyour application by hello Azure Access Control Service (ACS).</span></span> <span data-ttu-id="34a51-105">Průvodce Hello vychází hello [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) tématu, tím, že poskytuje kód, který zobrazí informace SAML hello.</span><span class="sxs-lookup"><span data-stu-id="34a51-105">hello guide builds on hello [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) topic, by providing code that displays hello SAML information.</span></span> <span data-ttu-id="34a51-106">aplikace Hello Dokončit bude vypadat podobně jako následující toohello.</span><span class="sxs-lookup"><span data-stu-id="34a51-106">hello completed application will look similar toohello following.</span></span>

![Příklad výstupu SAML][saml_output]

<span data-ttu-id="34a51-108">Další informace o ACS najdete v tématu hello [další kroky](#next_steps) části.</span><span class="sxs-lookup"><span data-stu-id="34a51-108">For more information on ACS, see hello [Next steps](#next_steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="34a51-109">Hello filtru řízení služeb Azure přístup je náhled technologie komunity.</span><span class="sxs-lookup"><span data-stu-id="34a51-109">hello Azure Access Services Control Filter is a community technology preview.</span></span> <span data-ttu-id="34a51-110">Jako předběžné verze softwaru není oficiálně společností Microsoft.</span><span class="sxs-lookup"><span data-stu-id="34a51-110">As pre-release software, it is not formally supported by Microsoft.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="34a51-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="34a51-111">Prerequisites</span></span>
<span data-ttu-id="34a51-112">toocomplete hello úlohy v této příručce, dokončení hello ukázku najdete na adrese [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) a používejte ho jako výchozí bod pro účely tohoto kurzu hello.</span><span class="sxs-lookup"><span data-stu-id="34a51-112">toocomplete hello tasks in this guide, complete hello sample at [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) and use it as hello starting point for this tutorial.</span></span>

## <a name="add-hello-jspwriter-library-tooyour-build-path-and-deployment-assembly"></a><span data-ttu-id="34a51-113">Přidat hello JspWriter tooyour sestavení a nasazení cesta sestavení knihovny</span><span class="sxs-lookup"><span data-stu-id="34a51-113">Add hello JspWriter library tooyour build path and deployment assembly</span></span>
<span data-ttu-id="34a51-114">Přidání hello knihovny, který obsahuje hello **javax.servlet.jsp.JspWriter** tooyour třída sestavení a nasazení cesta sestavení.</span><span class="sxs-lookup"><span data-stu-id="34a51-114">Add hello library that contains hello **javax.servlet.jsp.JspWriter** class tooyour build path and deployment assembly.</span></span> <span data-ttu-id="34a51-115">Pokud používáte Tomcat, je hello knihovně **jsp api.jar**, který se nachází v hello Apache **lib** složky.</span><span class="sxs-lookup"><span data-stu-id="34a51-115">If you are using Tomcat, hello library is **jsp-api.jar**, which is located in hello Apache **lib** folder.</span></span>

1. <span data-ttu-id="34a51-116">V prostředí Eclipse v prohlížeči projektu klikněte pravým tlačítkem na **MyACSHelloWorld**, klikněte na tlačítko **cesta sestavení**, klikněte na tlačítko **konfigurovat cestu sestavení**, klikněte na tlačítko hello **knihovny** a pak klikněte **přidat externí JARs**.</span><span class="sxs-lookup"><span data-stu-id="34a51-116">In Eclipse's Project Explorer, right-click **MyACSHelloWorld**, click **Build Path**, click **Configure Build Path**, click hello **Libraries** tab, and then click **Add External JARs**.</span></span>
2. <span data-ttu-id="34a51-117">V hello **JAR výběr** dialogové okno, přejděte toohello nezbytné JAR, vyberte ho a pak klikněte na tlačítko **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="34a51-117">In hello **JAR Selection** dialog, navigate toohello necessary JAR, select it, and then click **Open**.</span></span>
3. <span data-ttu-id="34a51-118">S hello **vlastnosti pro MyACSHelloWorld** stále otevřená, klikněte na dialogové okno **nasazení sestavení**.</span><span class="sxs-lookup"><span data-stu-id="34a51-118">With hello **Properties for MyACSHelloWorld** dialog still open, click **Deployment Assembly**.</span></span>
4. <span data-ttu-id="34a51-119">V hello **webové nasazení sestavení** dialogové okno, klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="34a51-119">In hello **Web Deployment Assembly** dialog, click **Add**.</span></span>
5. <span data-ttu-id="34a51-120">V hello **nové – Direktiva Assembly** dialogové okno, klikněte na tlačítko **položky cesta sestavení Java** a pak klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="34a51-120">In hello **New Assembly Directive** dialog, click **Java Build Path Entries** and then click **Next**.</span></span>
6. <span data-ttu-id="34a51-121">Vyberte hello příslušnou knihovnu a klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="34a51-121">Select hello appropriate library and click **Finish**.</span></span>
7. <span data-ttu-id="34a51-122">Klikněte na tlačítko **OK** tooclose hello **vlastnosti pro MyACSHelloWorld** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="34a51-122">Click **OK** tooclose hello **Properties for MyACSHelloWorld** dialog.</span></span>

## <a name="modify-hello-jsp-file-toodisplay-saml"></a><span data-ttu-id="34a51-123">Upravit toodisplay soubor JSP hello SAML</span><span class="sxs-lookup"><span data-stu-id="34a51-123">Modify hello JSP file toodisplay SAML</span></span>
<span data-ttu-id="34a51-124">Upravit **index.jsp** toouse hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="34a51-124">Modify **index.jsp** toouse hello following code.</span></span>

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print hello text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out hello child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate hello names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish hello sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process hello child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate hello child nodes of hello doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-hello-application"></a><span data-ttu-id="34a51-125">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="34a51-125">Run hello application</span></span>
1. <span data-ttu-id="34a51-126">Spusťte aplikaci v emulátoru hello počítače nebo nasadit tooAzure pomocí hello kroků popsaných v [jak tooAuthenticate webovým uživatelům s Azure přístup k řízení služby pomocí Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="34a51-126">Run your application in hello computer emulator or deploy tooAzure, using hello steps documented at [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span></span>
2. <span data-ttu-id="34a51-127">Spusťte prohlížeč a otevřete webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34a51-127">Launch a browser and open your web application.</span></span> <span data-ttu-id="34a51-128">Po přihlášení tooyour aplikace se zobrazí informace o SAML, včetně assertion hello zabezpečení poskytované poskytovatelem identity hello.</span><span class="sxs-lookup"><span data-stu-id="34a51-128">After you log on tooyour application, you'll see SAML information, including hello security assertion provided by hello identity provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34a51-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34a51-129">Next steps</span></span>
<span data-ttu-id="34a51-130">toofurther prozkoumat funkce služby ACS a tooexperiment se složitější scénáři najdete v tématu [2.0 služby Řízení přístupu][Access Control Service 2.0].</span><span class="sxs-lookup"><span data-stu-id="34a51-130">toofurther explore ACS's functionality and tooexperiment with more sophisticated scenarios, see [Access Control Service 2.0][Access Control Service 2.0].</span></span>

[Prerequisites]: #pre
[Modify hello JSP file toodisplay SAML]: #modify_jsp
[Add hello JspWriter library tooyour build path and deployment assembly]: #add_library
[Run hello application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
