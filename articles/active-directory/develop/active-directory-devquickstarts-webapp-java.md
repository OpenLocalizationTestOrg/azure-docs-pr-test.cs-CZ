---
title: "Webové aplikace Azure AD Java Začínáme | Microsoft Docs"
description: "Vytvořte webovou aplikaci Java, která podepisuje uživatele přihlašují pomocí pracovního nebo školního účtu."
services: active-directory
documentationcenter: java
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 2b92b605-9cd5-4b99-bcbb-66c026558119
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 02/01/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 5358404881b65d217ab36a41ca04a73f2c462c86
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="java-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="da1d9-103">Webové aplikace v jazyce Java přihlášení a odhlášení s Azure AD</span><span class="sxs-lookup"><span data-stu-id="da1d9-103">Java web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="da1d9-104">Tím, že poskytuje jeden přihlášení a odhlášení jenom pár řádků kódu, Azure Active Directory (Azure AD) umožňuje snadno můžete využít Správa identit pro webové aplikace k.</span><span class="sxs-lookup"><span data-stu-id="da1d9-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you to outsource web-app identity management.</span></span> <span data-ttu-id="da1d9-105">Uživatelé a deaktivovat webové aplikace Java můžete podepsat pomocí implementaci společnosti Microsoft základě komunity Azure Active Directory Authentication Library pro jazyk Java (ADAL4J).</span><span class="sxs-lookup"><span data-stu-id="da1d9-105">You can sign users in and out of Java web apps by using the Microsoft implementation of the community-driven Azure Active Directory Authentication Library for Java (ADAL4J).</span></span>

<span data-ttu-id="da1d9-106">Tento článek ukazuje, jak používat ADAL4J na:</span><span class="sxs-lookup"><span data-stu-id="da1d9-106">This article shows how to use the ADAL4J to:</span></span>

* <span data-ttu-id="da1d9-107">Přihlášení uživatelů k webové aplikace pomocí Azure AD jako zprostředkovatele identity.</span><span class="sxs-lookup"><span data-stu-id="da1d9-107">Sign users in to web apps by using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="da1d9-108">Zobrazí některé informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="da1d9-108">Display some user information.</span></span>
* <span data-ttu-id="da1d9-109">Přihlášení uživatelů z aplikace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-109">Sign users out of the apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="da1d9-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="da1d9-110">Before you get started</span></span>

* <span data-ttu-id="da1d9-111">Stažení [kostru aplikace](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip), nebo stáhnout [hotová ukázka](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="da1d9-111">Download the [app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip), or download the [completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>
* <span data-ttu-id="da1d9-112">Musíte taky klient služby Azure AD, ve kterém pro registraci aplikace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-112">You also need an Azure AD tenant in which to register the app.</span></span> <span data-ttu-id="da1d9-113">Pokud ještě nemáte klient služby Azure AD [zjistěte, jak získat](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="da1d9-113">If you don't already have an Azure AD tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="da1d9-114">Až budete připravení, postupujte podle pokynů v další devět částí.</span><span class="sxs-lookup"><span data-stu-id="da1d9-114">When you are ready, follow the procedures in the next nine sections.</span></span>

## <a name="step-1-register-the-new-app-with-azure-ad"></a><span data-ttu-id="da1d9-115">Krok 1: Zaregistrujte novou aplikaci s Azure AD</span><span class="sxs-lookup"><span data-stu-id="da1d9-115">Step 1: Register the new app with Azure AD</span></span>
<span data-ttu-id="da1d9-116">Chcete-li nastavit aplikaci pro ověřování uživatelů, nejprve zaregistrovat ve vašem klientovi provedením následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="da1d9-116">To set up the app to authenticate users, first register it in your tenant by doing the following:</span></span>

1. <span data-ttu-id="da1d9-117">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="da1d9-117">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="da1d9-118">Na horním panelu klikněte na název účtu.</span><span class="sxs-lookup"><span data-stu-id="da1d9-118">On the top bar, click your account name.</span></span> <span data-ttu-id="da1d9-119">V části **Directory** vyberte klienta služby Active Directory, ve které chcete zaregistrovat aplikaci.</span><span class="sxs-lookup"><span data-stu-id="da1d9-119">Under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="da1d9-120">Klikněte na tlačítko **více služeb** v levém podokně a potom vyberte **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="da1d9-120">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="da1d9-121">Klikněte na tlačítko **registrace aplikace**a potom vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="da1d9-121">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="da1d9-122">Postupujte podle výzev a vytvořte **webové aplikace nebo WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="da1d9-122">Follow the prompts to create a **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="da1d9-123">**Název** popis aplikace pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="da1d9-123">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="da1d9-124">**Adresa URL přihlašování** je základní adresu URL aplikace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-124">**Sign-On URL** is the base URL of the app.</span></span> <span data-ttu-id="da1d9-125">Je kostře výchozí adresa URL adrese http://localhost: 8080/adal4jsample /.</span><span class="sxs-lookup"><span data-stu-id="da1d9-125">The skeleton's default URL is http://localhost:8080/adal4jsample/.</span></span>
6. <span data-ttu-id="da1d9-126">Po dokončení registrace Azure AD přiřadí aplikace ID jedinečný aplikace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-126">After you've completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="da1d9-127">Zkopírujte hodnotu na stránce aplikace používat v dalších částech.</span><span class="sxs-lookup"><span data-stu-id="da1d9-127">Copy the value from the app page to use in the next sections.</span></span>
7. <span data-ttu-id="da1d9-128">Z **nastavení** -> **vlastnosti** stránky pro aplikace, aktualizujte identifikátor ID URI aplikace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-128">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="da1d9-129">**Identifikátor ID URI aplikace** je jedinečný identifikátor pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="da1d9-129">The **App ID URI** is a unique identifier for the app.</span></span> <span data-ttu-id="da1d9-130">Zásady vytváření názvů je `https://<tenant-domain>/<app-name>` (například `http://localhost:8080/adal4jsample/`).</span><span class="sxs-lookup"><span data-stu-id="da1d9-130">The naming convention is `https://<tenant-domain>/<app-name>` (for example, `http://localhost:8080/adal4jsample/`).</span></span>

<span data-ttu-id="da1d9-131">Pokud jste na portálu pro aplikaci, vytvořte a zkopírujte klíč pro aplikaci na **nastavení** stránky.</span><span class="sxs-lookup"><span data-stu-id="da1d9-131">When you are in the portal for the app, create and copy a key for the app on the **Settings** page.</span></span> <span data-ttu-id="da1d9-132">Klíč budete potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="da1d9-132">You'll need the key shortly.</span></span>

## <a name="step-2-set-up-the-app-to-use-the-adal4j-and-prerequisites-by-using-maven"></a><span data-ttu-id="da1d9-133">Krok 2: Nastavení aplikace pro použití ADAL4J a požadavky s využitím Maven</span><span class="sxs-lookup"><span data-stu-id="da1d9-133">Step 2: Set up the app to use the ADAL4J and prerequisites by using Maven</span></span>
<span data-ttu-id="da1d9-134">V tomto kroku nakonfigurujete ADAL4J pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="da1d9-134">In this step, you configure the ADAL4J to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="da1d9-135">ADAL4J slouží k vydávání požadavků na přihlášení a odhlášení, správě uživatelských relací, získání informací o uživateli a tak dále.</span><span class="sxs-lookup"><span data-stu-id="da1d9-135">You use the ADAL4J to issue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

<span data-ttu-id="da1d9-136">V kořenovém adresáři projektu, otevírání nebo vytváření `pom.xml`, vyhledejte `// TODO: provide dependencies for Maven`a nahraďte ji následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="da1d9-136">In the root directory of your project, open/create `pom.xml`, locate `// TODO: provide dependencies for Maven`, and replace it with the following:</span></span>

```Java

    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4jsample</artifactId>
    <packaging>war</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>adal4jsample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.1</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
        <!-- Spring 3 dependencies -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>sample-for-adal4j</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <warName>${project.artifactId}</warName>
                    <source>${project.basedir}\src</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>utf-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
    </build>

    </project>
```

## <a name="step-3-create-the-java-web-app-files-web-inf"></a><span data-ttu-id="da1d9-137">Krok 3: Vytvoření souborů webové aplikace Java (WEB-INF)</span><span class="sxs-lookup"><span data-stu-id="da1d9-137">Step 3: Create the Java web app files (WEB-INF)</span></span>
<span data-ttu-id="da1d9-138">V tomto kroku nakonfigurujete webové aplikace Java pro použití ověřovacího protokolu OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="da1d9-138">In this step, you configure the Java web app to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="da1d9-139">Pomocí ADAL4J zasílání požadavků na přihlášení a odhlášení, spravovat relace uživatele, získat informace o uživateli a tak dále.</span><span class="sxs-lookup"><span data-stu-id="da1d9-139">Use the ADAL4J to issue sign-in and sign-out requests, manage the user's session, get information about the user, and so forth.</span></span>

1. <span data-ttu-id="da1d9-140">Otevřete soubor web.xml přidávaném \webapp\WEB-INF\, a zadejte hodnoty konfigurace aplikace v souboru XML.</span><span class="sxs-lookup"><span data-stu-id="da1d9-140">Open the web.xml file located under \webapp\WEB-INF\, and enter the app configuration values in the XML.</span></span> <span data-ttu-id="da1d9-141">Soubor XML musí obsahovat následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-141">The XML file should contain the following code:</span></span>

    ```xml

    <?xml version="1.0"?>
    <web-app id="WebApp_ID" version="2.4"
        xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee
        http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
        <display-name>Archetype Created Web Application</display-name>
        <context-param>
            <param-name>authority</param-name>
            <param-value>https://login.microsoftonline.com/</param-value>
        </context-param>
        <context-param>
            <param-name>tenant</param-name>
            <param-value>YOUR_TENANT_NAME</param-value>
        </context-param>
        <filter>
            <filter-name>BasicFilter</filter-name>
            <filter-class>com.microsoft.aad.adal4jsample.BasicFilter</filter-class>
            <init-param>
                <param-name>client_id</param-name>
                <param-value>YOUR_CLIENT_ID</param-value>
            </init-param>
            <init-param>
                <param-name>secret_key</param-name>
                <param-value>YOUR_CLIENT_SECRET</param-value>
            </init-param>
        </filter>
        <filter-mapping>
            <filter-name>BasicFilter</filter-name>
            <url-pattern>/secure/*</url-pattern>
        </filter-mapping>
        <servlet>
            <servlet-name>mvc-dispatcher</servlet-name>
            <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
            <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
            <servlet-name>mvc-dispatcher</servlet-name>
            <url-pattern>/</url-pattern>
        </servlet-mapping>
        <context-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/mvc-dispatcher-servlet.xml</param-value>
        </context-param>
        <listener>
            <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
        </listener>
    </web-app>
    ```

 * <span data-ttu-id="da1d9-142">Je YOUR_CLIENT_ID **Id aplikace** přiřazené vaší aplikaci v portálu pro registraci.</span><span class="sxs-lookup"><span data-stu-id="da1d9-142">YOUR_CLIENT_ID is the **Application Id** assigned to your app in the registration portal.</span></span>
 * <span data-ttu-id="da1d9-143">Je YOUR_CLIENT_SECRET **tajný klíč aplikace** vytvořený na portálu.</span><span class="sxs-lookup"><span data-stu-id="da1d9-143">YOUR_CLIENT_SECRET is the **Application Secret** that you created in the portal.</span></span>
 * <span data-ttu-id="da1d9-144">Je YOUR_TENANT_NAME **název klienta** vaší aplikace (například contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="da1d9-144">YOUR_TENANT_NAME is the **tenant name** of your app (for example, contoso.onmicrosoft.com).</span></span>

 <span data-ttu-id="da1d9-145">Jak vidíte v souboru XML, píšete názvem mvc dispečera, který používá BasicFilter vždy, když navštívíte webovou aplikaci JavaServer stránky (JSP) nebo Java Servlet / secure adresy URL.</span><span class="sxs-lookup"><span data-stu-id="da1d9-145">As you can see in the XML file, you are writing a JavaServer Pages (JSP) or Java Servlet web app called mvc-dispatcher that uses BasicFilter whenever you visit the /secure URL.</span></span> <span data-ttu-id="da1d9-146">V stejný kód použijte / secure jako místo pro chráněný obsah a k vynucení ověřování do služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da1d9-146">In the same code, you use /secure as a place for the protected content and to force authentication to Azure AD.</span></span>

2. <span data-ttu-id="da1d9-147">Vytvoření souboru mvc. dispečera servlet.xml přidávaném \webapp\WEB-INF\, a zadejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-147">Create the mvc-dispatcher-servlet.xml file located under \webapp\WEB-INF\, and enter the following code:</span></span>

    ```xml

    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="
            http://www.springframework.org/schema/beans     
            http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
            http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context-3.0.xsd">
        <context:component-scan base-package="com.microsoft.aad.adal4jsample" />
        <bean
            class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix">
                <value>/</value>
            </property>
            <property name="suffix">
                <value>.jsp</value>
            </property>
        </bean>
    </beans>
    ```

 <span data-ttu-id="da1d9-148">Tento kód informuje webovou aplikaci k používání pružiny a ukazuje, kde najít soubor JSP, který můžete psát v další části.</span><span class="sxs-lookup"><span data-stu-id="da1d9-148">This code tells the web app to use Spring, and it indicates where to find the JSP file, which you write in the next section.</span></span>

## <a name="step-4-create-the-jsp-view-files-for-basicfilter-mvc"></a><span data-ttu-id="da1d9-149">Krok 4: Vytvoření JSP zobrazit soubory (pro BasicFilter MVC)</span><span class="sxs-lookup"><span data-stu-id="da1d9-149">Step 4: Create the JSP View files (for BasicFilter MVC)</span></span>
<span data-ttu-id="da1d9-150">Jsou poloviny prostřednictvím nastavení webové aplikace na WEB INF.</span><span class="sxs-lookup"><span data-stu-id="da1d9-150">You are half-way through setting up your web app in WEB-INF.</span></span> <span data-ttu-id="da1d9-151">Dále vytvoříte JSP soubory pro BasicFilter modelu zobrazení controller (MVC), která zpracuje webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="da1d9-151">Next, you create the JSP files for BasicFilter model view controller (MVC), which the web app executes.</span></span> <span data-ttu-id="da1d9-152">Jsme s instrukcemi při vytváření souborů během konfigurace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-152">We hinted at creating the files during the configuration.</span></span>

<span data-ttu-id="da1d9-153">Dříve, je v aplikaci Java v konfiguračních souborech XML, které máte `/` prostředek, který načte JSP soubory a budete mít `/secure` prostředek, který prochází filtr, který jste volali metodu BasicFilter.</span><span class="sxs-lookup"><span data-stu-id="da1d9-153">Earlier, you told Java in the XML configuration files that you have a `/` resource that loads JSP files, and you have a `/secure` resource that passes through a filter, which you called BasicFilter.</span></span>

<span data-ttu-id="da1d9-154">Pokud chcete vytvořit soubory JSP, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="da1d9-154">To create the JSP files, do the following:</span></span>

1. <span data-ttu-id="da1d9-155">Vytvořit soubor index.jsp (umístěné v \webapp\)a potom vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-155">Create the index.jsp file (located under \webapp\), and then paste the following code:</span></span>

    ```jsp
    <html>
    <body>
        <h2>Hello World!</h2>
        <ul>
        <li><a href="secure/aad">Secure Page</a></li>
        </ul>
    </body>
    </html>
    ```

 <span data-ttu-id="da1d9-156">Tento kód jednoduše přesměruje na zabezpečené stránky, který je chráněný pomocí filtru.</span><span class="sxs-lookup"><span data-stu-id="da1d9-156">This code simply redirects to a secure page that is protected by the filter.</span></span>

2. <span data-ttu-id="da1d9-157">Ve stejném adresáři vytvořte soubor error.jsp k zachycení všechny chyby, které by se mohlo stát:</span><span class="sxs-lookup"><span data-stu-id="da1d9-157">In the same directory, create an error.jsp file to catch any errors that might happen:</span></span>

    ```jsp

    <html>
    <body>
        <h2>ERROR PAGE!</h2>
        <p>
            Exception -
            <%=request.getAttribute("error")%></p>
        <ul>
            <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
        </ul>
    </body>
    </html>
    ```
3. <span data-ttu-id="da1d9-158">Chcete-li zabezpečené webová stránka, vytvořte složku s názvem \secure tak, aby adresáře je nyní \webapp\secure \webapp.</span><span class="sxs-lookup"><span data-stu-id="da1d9-158">To make that secure webpage, create a folder under \webapp called \secure so that the directory is now \webapp\secure.</span></span>
4. <span data-ttu-id="da1d9-159">V adresáři \webapp\secure vytvořte soubor aad.jsp a vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-159">In the \webapp\secure directory, create an aad.jsp file, and then paste the following code:</span></span>

    ```jsp

    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
        <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>AAD Secure Page</title>
        </head>
        <body>

        <h1>Directory - Users List</h1>
        <p>${users}</p>
        <ul>
            <li><a href="<%=request.getContextPath()%>/secure/aad?cc=1">Get new Access Token via Client Credentials</a></li>
        </ul>
        <ul>
            <li><a href="<%=request.getContextPath()%>/secure/aad?refresh=1">Get new Access Token via Refresh Token</a></li>
        </ul>
        <ul>
            <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
        </ul>
        </body>
    </html>
    ```

    <span data-ttu-id="da1d9-160">Tato stránka se přesměruje na konkrétní požadavky, které se servletem BasicFilter přečte a pak je prováděn pomocí ADAJ4J.</span><span class="sxs-lookup"><span data-stu-id="da1d9-160">This page redirects to specific requests, which the BasicFilter servlet reads and then executes on by using the ADAJ4J.</span></span>

<span data-ttu-id="da1d9-161">Teď je potřeba nastavit Java soubory tak, aby se servletem můžete dělat svou práci.</span><span class="sxs-lookup"><span data-stu-id="da1d9-161">You now need to set up the Java files so that the servlet can do its work.</span></span>

## <a name="step-5-create-some-java-helper-files-for-basicfilter-mvc"></a><span data-ttu-id="da1d9-162">Krok 5: Vytvoření některé soubory pomocná Java (pro BasicFilter MVC)</span><span class="sxs-lookup"><span data-stu-id="da1d9-162">Step 5: Create some Java helper files (for BasicFilter MVC)</span></span>
<span data-ttu-id="da1d9-163">Naším cílem v tomto kroku je vytvoření Java soubory, které:</span><span class="sxs-lookup"><span data-stu-id="da1d9-163">Our goal in this step is to create Java files that:</span></span>

* <span data-ttu-id="da1d9-164">Povolte pro přihlášení a odhlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="da1d9-164">Allow for sign-in and sign-out of the user.</span></span>
* <span data-ttu-id="da1d9-165">Získáte některá data o uživateli.</span><span class="sxs-lookup"><span data-stu-id="da1d9-165">Get some data about the user.</span></span>

    > [!NOTE]
    > <span data-ttu-id="da1d9-166">Data o uživateli, použijte rozhraní Graph API z Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da1d9-166">To get data about the user, use the Graph API from Azure AD.</span></span> <span data-ttu-id="da1d9-167">Rozhraní Graph API je zabezpečení webové služby, který vám pomůže získat data o vaší organizaci, včetně jednotlivých uživatelů.</span><span class="sxs-lookup"><span data-stu-id="da1d9-167">The Graph API is a secure webservice that you can use to grab data about your organization, including individual users.</span></span> <span data-ttu-id="da1d9-168">Tento přístup je lepší, než předem naplnění citlivá data v tokenech, protože zajišťuje, aby:</span><span class="sxs-lookup"><span data-stu-id="da1d9-168">This approach is better than pre-filling sensitive data in tokens, because it ensures that:</span></span>
    > * <span data-ttu-id="da1d9-169">Uživatelé, kteří požádat o data mají oprávnění.</span><span class="sxs-lookup"><span data-stu-id="da1d9-169">The users who ask for the data are authorized.</span></span>
    > * <span data-ttu-id="da1d9-170">Každý, kdo může dojít k získání tokenu (z jailbreak telefon nebo webový prohlížeč mezipaměti na plochu, např.) nelze získat důležité podrobnosti o uživatele nebo organizace.</span><span class="sxs-lookup"><span data-stu-id="da1d9-170">Anyone who might happen to grab the token (from a jailbroken phone or web-browser cache on a desktop, for example) cannot obtain important details about the user or the organization.</span></span>

<span data-ttu-id="da1d9-171">Zápis některé soubory Java pro tento pracovní:</span><span class="sxs-lookup"><span data-stu-id="da1d9-171">To write some Java files for this work:</span></span>

1. <span data-ttu-id="da1d9-172">Vytvořte složku v kořenovém adresáři názvem adal4jsample uložení všech souborů Java.</span><span class="sxs-lookup"><span data-stu-id="da1d9-172">Create a folder in your root directory called adal4jsample to store all the Java files.</span></span>

    <span data-ttu-id="da1d9-173">V tomto příkladu se používají v souborech Java com.microsoft.aad.adal4jsample oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="da1d9-173">In this example, you are using the namespace com.microsoft.aad.adal4jsample in the Java files.</span></span> <span data-ttu-id="da1d9-174">Většina integrovaného vývojového prostředí vytvořit vnořenou složku strukturu pro tento účel (například /com/microsoft/aad/adal4jsample).</span><span class="sxs-lookup"><span data-stu-id="da1d9-174">Most IDEs create a nested folder structure for this purpose (for example, /com/microsoft/aad/adal4jsample).</span></span> <span data-ttu-id="da1d9-175">Můžete to provést také, ale není nutné.</span><span class="sxs-lookup"><span data-stu-id="da1d9-175">You can do this also, but it is not necessary.</span></span>

2. <span data-ttu-id="da1d9-176">V této složce vytvořte soubor s názvem JSONHelper.java, které budete používat pomohou analyzovat data JSON z vašich tokenů.</span><span class="sxs-lookup"><span data-stu-id="da1d9-176">Inside this folder, create a file called JSONHelper.java, which you'll use to help parse the JSON data from your tokens.</span></span> <span data-ttu-id="da1d9-177">Pokud chcete vytvořit soubor, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-177">To create the file, paste the following code:</span></span>

    ```Java

    package com.microsoft.aad.adal4jsample;

    import java.lang.reflect.Field;
    import java.util.Arrays;
    import java.util.Enumeration;
    import java.util.List;

    import javax.servlet.http.HttpServletRequest;

    import org.apache.commons.lang3.text.WordUtils;
    import org.apache.log4j.Logger;
    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;

    /**
     * This class provides the methods to parse JSON data from a JSON-formatted
     * string.
     *
     * @author Azure Active Directory contributor
     *
     */
    public class JSONHelper {

        private static Logger logger = Logger.getLogger(JSONHelper.class);

        JSONHelper() {
            // PropertyConfigurator.configure("log4j.properties");
        }

        /**
         * This method parses a JSON array out of a collection of JSON objects
         * within a string.
         *
         * @param jSonData
         *            The JSON string that holds the collection
         * @return A JSON array that contains all the collection objects
         * @throws Exception
         */
        public static JSONArray fetchDirectoryObjectJSONArray(JSONObject jsonObject) throws Exception {
            JSONArray jsonArray = new JSONArray();
            jsonArray = jsonObject.optJSONObject("responseMsg").optJSONArray("value");
            return jsonArray;
        }

        /**
         * This method parses a JSON object out of a collection of JSON objects
         * within a string.
         *
         * @param jsonObject
         * @return A JSON object that contains the DirectoryObject
         * @throws Exception
         */
        public static JSONObject fetchDirectoryObjectJSONObject(JSONObject jsonObject) throws Exception {
            JSONObject jObj = new JSONObject();
            jObj = jsonObject.optJSONObject("responseMsg");
            return jObj;
        }

        /**
         * This method parses the skip token from a JSON-formatted string.
         *
         * @param jsonData
         *            The JSON-formatted string
         * @return The skipToken
         * @throws Exception
         */
        public static String fetchNextSkiptoken(JSONObject jsonObject) throws Exception {
            String skipToken = "";
            // Parse the skip token out of the string.
            skipToken = jsonObject.optJSONObject("responseMsg").optString("odata.nextLink");

            if (!skipToken.equalsIgnoreCase("")) {
                // Remove the unnecessary prefix from the skip token.
                int index = skipToken.indexOf("$skiptoken=") + (new String("$skiptoken=")).length();
                skipToken = skipToken.substring(index);
            }
            return skipToken;
        }

        /**
         * @param jsonObject
         * @return
         * @throws Exception
         */
        public static String fetchDeltaLink(JSONObject jsonObject) throws Exception {
            String deltaLink = "";
            // Parse the skip token out of the string.
            deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.deltaLink");
            if (deltaLink == null || deltaLink.length() == 0) {
                deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.nextLink");
                logger.info("deltaLink empty, nextLink ->" + deltaLink);

            }
            if (!deltaLink.equalsIgnoreCase("")) {
                // Remove the unnecessary prefix from the skip token.
                int index = deltaLink.indexOf("deltaLink=") + (new String("deltaLink=")).length();
                deltaLink = deltaLink.substring(index);
            }
            return deltaLink;
        }

        /**
         * This method creates a string consisting of a JSON document with all
         * the necessary elements set from the HttpServletRequest request.
         *
         * @param request
         *            The HttpServletRequest
         * @return The string containing the JSON document
         * @throws Exception
         *             If there is any error processing the request.
         */
        public static String createJSONString(HttpServletRequest request, String controller) throws Exception {
            JSONObject obj = new JSONObject();
            try {
                Field[] allFields = Class.forName(
                        "com.microsoft.windowsazure.activedirectory.sdk.graph.models." + controller).getDeclaredFields();
                String[] allFieldStr = new String[allFields.length];
                for (int i = 0; i < allFields.length; i++) {
                    allFieldStr[i] = allFields[i].getName();
                }
                List<String> allFieldStringList = Arrays.asList(allFieldStr);
                Enumeration<String> fields = request.getParameterNames();

                while (fields.hasMoreElements()) {

                    String fieldName = fields.nextElement();
                    String param = request.getParameter(fieldName);
                    if (allFieldStringList.contains(fieldName)) {
                        if (param == null || param.length() == 0) {
                            if (!fieldName.equalsIgnoreCase("password")) {
                                obj.put(fieldName, JSONObject.NULL);
                            }
                        } else {
                            if (fieldName.equalsIgnoreCase("password")) {
                                obj.put("passwordProfile", new JSONObject("{\"password\": \"" + param + "\"}"));
                            } else {
                                obj.put(fieldName, param);
                            }
                        }
                    }
                }
            } catch (JSONException e) {
                e.printStackTrace();
            } catch (SecurityException e) {
                e.printStackTrace();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }            
            return obj.toString();
        }

        /**
         *
         * @param key
         * @param value
         * @return string format of this JSON object
         * @throws Exception
         */
        public static String createJSONString(String key, String value) throws Exception {

            JSONObject obj = new JSONObject();
            try {
                obj.put(key, value);
            } catch (JSONException e) {
                e.printStackTrace();
            }

            return obj.toString();
        }

        /**
         * This is a generic method that copies the simple attribute values from an
         * argument jsonObject to an argument generic object.
         *
         * @param jsonObject
         *            The jsonObject from where the attributes are to be copied.
         * @param destObject
         *            The object where the attributes should be copied to.
         * @throws Exception
         *             Throws an Exception when the operation is unsuccessful.
         */
        public static <T> void convertJSONObjectToDirectoryObject(JSONObject jsonObject, T destObject) throws Exception {

            // Get the list of all the field names.
            Field[] fieldList = destObject.getClass().getDeclaredFields();

            // For all the declared field.
            for (int i = 0; i < fieldList.length; i++) {
                // If the field is of type String, that is
                // if it is a simple attribute.
                if (fieldList[i].getType().equals(String.class)) {
                    // Invoke the corresponding set method of the destObject using
                    // the argument taken from the jsonObject.
                    destObject
                            .getClass()
                            .getMethod(String.format("set%s", WordUtils.capitalize(fieldList[i].getName())),
                                    new Class[] { String.class })
                            .invoke(destObject, new Object[] { jsonObject.optString(fieldList[i].getName()) });
                }
            }
        }

        public static JSONArray joinJSONArrays(JSONArray a, JSONArray b) {
            JSONArray comb = new JSONArray();
            for (int i = 0; i < a.length(); i++) {
                comb.put(a.optJSONObject(i));
            }
            for (int i = 0; i < b.length(); i++) {
                comb.put(b.optJSONObject(i));
            }
            return comb;
        }

    }

    ```

3. <span data-ttu-id="da1d9-178">Vytvořte soubor s názvem HttpClientHelper.java, které budete používat pomohou analyzovat data protokolu HTTP z koncový bod služby Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da1d9-178">Create a file called HttpClientHelper.java, which you will use to help parse the HTTP data from your Azure AD endpoint.</span></span> <span data-ttu-id="da1d9-179">Pokud chcete vytvořit soubor, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-179">To create the file, paste the following code:</span></span>

    ```Java

    package com.microsoft.aad.adal4jsample;

    import java.io.BufferedReader;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.InputStreamReader;
    import java.io.OutputStreamWriter;
    import java.net.HttpURLConnection;

    import org.json.JSONException;
    import org.json.JSONObject;

    /**
     * This is Helper class for all RestClient class.
     *
     * @author Azure Active Directory Contributor
     *
     */
    public class HttpClientHelper {

        public HttpClientHelper() {
            super();
        }

        public static String getResponseStringFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

            BufferedReader reader = null;
            if (isSuccess) {
                reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            } else {
                reader = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
            }
            StringBuffer stringBuffer = new StringBuffer();
            String line = "";
            while ((line = reader.readLine()) != null) {
                stringBuffer.append(line);
            }

            return stringBuffer.toString();
        }

        public static String getResponseStringFromConn(HttpURLConnection conn, String payLoad) throws IOException {

            // Send the http message payload to the server.
            if (payLoad != null) {
                conn.setDoOutput(true);
                OutputStreamWriter osw = new OutputStreamWriter(conn.getOutputStream());
                osw.write(payLoad);
                osw.flush();
                osw.close();
            }

            // Get the message response from the server.
            BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String line = "";
            StringBuffer stringBuffer = new StringBuffer();
            while ((line = br.readLine()) != null) {
                stringBuffer.append(line);
            }

            br.close();

            return stringBuffer.toString();
        }

        public static byte[] getByteaArrayFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

            InputStream is = conn.getInputStream();
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            byte[] buff = new byte[1024];
            int bytesRead = 0;

            while ((bytesRead = is.read(buff, 0, buff.length)) != -1) {
                baos.write(buff, 0, bytesRead);
            }

            byte[] bytes = baos.toByteArray();
            baos.close();
            return bytes;
        }

        /**
         * for bad response, whose responseCode is not 200 level
         *
         * @param responseCode
         * @param errorCode
         * @param errorMsg
         * @return
         * @throws JSONException
         */
        public static JSONObject processResponse(int responseCode, String errorCode, String errorMsg) throws JSONException {
            JSONObject response = new JSONObject();
            response.put("responseCode", responseCode);
            response.put("errorCode", errorCode);
            response.put("errorMsg", errorMsg);

            return response;
        }

        /**
         * for bad response, whose responseCode is not 200 level
         *
         * @param responseCode
         * @param errorCode
         * @param errorMsg
         * @return
         * @throws JSONException
         */
        public static JSONObject processGoodRespStr(int responseCode, String goodRespStr) throws JSONException {
            JSONObject response = new JSONObject();
            response.put("responseCode", responseCode);
            if (goodRespStr.equalsIgnoreCase("")) {
                response.put("responseMsg", "");
            } else {
                response.put("responseMsg", new JSONObject(goodRespStr));
            }

            return response;
        }

        /**
         * for good response
         *
         * @param responseCode
         * @param responseMsg
         * @return
         * @throws JSONException
         */
        public static JSONObject processBadRespStr(int responseCode, String responseMsg) throws JSONException {

            JSONObject response = new JSONObject();
            response.put("responseCode", responseCode);
            if (responseMsg.equalsIgnoreCase("")) { // good response is empty string
                response.put("responseMsg", "");
            } else { // bad response is json string
                JSONObject errorObject = new JSONObject(responseMsg).optJSONObject("odata.error");

                String errorCode = errorObject.optString("code");
                String errorMsg = errorObject.optJSONObject("message").optString("value");
                response.put("responseCode", responseCode);
                response.put("errorCode", errorCode);
                response.put("errorMsg", errorMsg);
            }

            return response;
        }

    }

    ```

## <a name="step-6-create-the-java-graph-api-model-files-for-basicfilter-mvc"></a><span data-ttu-id="da1d9-180">Krok 6: Vytvoření souborů Java Graph API Model (pro BasicFilter MVC)</span><span class="sxs-lookup"><span data-stu-id="da1d9-180">Step 6: Create the Java Graph API Model files (for BasicFilter MVC)</span></span>
<span data-ttu-id="da1d9-181">Jak je uvedeno dříve, použijete rozhraní Graph API k získání dat o přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="da1d9-181">As indicated previously, you use the Graph API to get data about the signed-in user.</span></span> <span data-ttu-id="da1d9-182">Chcete-li tento proces snadno, vytvořte soubor k reprezentaci objektu adresáře a souboru představující uživatele, aby bylo možné použít vzor ú jazyka Java.</span><span class="sxs-lookup"><span data-stu-id="da1d9-182">To make this process easy, create both a file to represent a Directory object and a file to represent the user so that the OO pattern of Java can be used.</span></span>

1. <span data-ttu-id="da1d9-183">Vytvořte soubor s názvem DirectoryObject.java, který použijete k uložení základní data o libovolný objekt adresáře.</span><span class="sxs-lookup"><span data-stu-id="da1d9-183">Create a file called DirectoryObject.java, which you use to store basic data about any Directory object.</span></span> <span data-ttu-id="da1d9-184">Tento soubor můžete později použít pro jiné grafu dotazy, které může provádět.</span><span class="sxs-lookup"><span data-stu-id="da1d9-184">You can use this file later for any other Graph queries you might perform.</span></span> <span data-ttu-id="da1d9-185">Pokud chcete vytvořit soubor, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-185">To create the file, paste the following code:</span></span>

    ```Java

    package com.microsoft.aad.adal4jsample;

    /**
     * @author Azure Active Directory Contributor
     *
     */
    public abstract class DirectoryObject {

        public DirectoryObject() {
            super();
        }

        /**
         *
         * @return
         */
        public abstract String getObjectId();

        /**
         * @param objectId
         */
        public abstract void setObjectId(String objectId);

        /**
         *
         * @return
         */
        public abstract String getObjectType();

        /**
         *
         * @param objectType
         */
        public abstract void setObjectType(String objectType);

        /**
         *
         * @return
         */
        public abstract String getDisplayName();

        /**
         *
         * @param displayName
         */
        public abstract void setDisplayName(String displayName);

    }

    ```

2. <span data-ttu-id="da1d9-186">Vytvořte soubor s názvem User.java, který použijete k uložení základní data o všechny uživatele z adresáře.</span><span class="sxs-lookup"><span data-stu-id="da1d9-186">Create a file called User.java, which you use to store basic data about any user from the directory.</span></span> <span data-ttu-id="da1d9-187">Tyto jsou základní metody getter a setter metody pro adresář data, takže můžete vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-187">These are basic getter and setter methods for directory data, so you can paste the following code:</span></span>

    ```Java

    package com.microsoft.aad.adal4jsample;

    import java.security.acl.Group;
    import java.util.ArrayList;

    import javax.xml.bind.annotation.XmlRootElement;

    import org.json.JSONObject;

    /**
    *  The **User** class holds together all the members of a WAAD User entity and all the access methods and set methods.
    *  @author Azure Active Directory Contributor
    */
    @XmlRootElement
    public class User extends DirectoryObject{

        // The following are the individual private members of a User object that holds
        // a particular simple attribute of a User object.
        protected String objectId;
        protected String objectType;
        protected String accountEnabled;
        protected String city;
        protected String country;
        protected String department;
        protected String dirSyncEnabled;
        protected String displayName;
        protected String facsimileTelephoneNumber;
        protected String givenName;
        protected String jobTitle;
        protected String lastDirSyncTime;
        protected String mail;
        protected String mailNickname;
        protected String mobile;
        protected String password;
        protected String passwordPolicies;
        protected String physicalDeliveryOfficeName;
        protected String postalCode;
        protected String preferredLanguage;
        protected String state;
        protected String streetAddress;
        protected String surname;
        protected String telephoneNumber;
        protected String usageLocation;
        protected String userPrincipalName;
        protected boolean isDeleted;  // this will move to dto

        /**
         * These four properties are for future use.
         */
        // managerDisplayname of this user.
        protected String managerDisplayname;

        // The directReports holds a list of directReports.
        private ArrayList<User> directReports;

        // The groups holds a list of group entities this user belongs to.
        private ArrayList<Group> groups;

        // The roles holds a list of role entities this user belongs to.
        private ArrayList<Group> roles;

        /**
         * The constructor for the **User** class. Initializes the dynamic lists and managerDisplayname variables.
         */
        public User(){
            directReports = null;
            groups = new ArrayList<Group>();
            roles = new ArrayList<Group>();
            managerDisplayname = null;
        }
    //    
    //    public User(String displayName, String objectId){
    //        setDisplayName(displayName);
    //        setObjectId(objectId);
    //    }
    //    
    //    public User(String displayName, String objectId, String userPrincipalName, String accountEnabled){
    //        setDisplayName(displayName);
    //        setObjectId(objectId);
    //        setUserPrincipalName(userPrincipalName);
    //        setAccountEnabled(accountEnabled);
    //    }
    //    

        /**
         * @return The objectId of this user.
         */
        public String getObjectId() {
            return objectId;
        }

        /**
         * @param objectId The objectId to set to this User object.
         */
        public void setObjectId(String objectId) {
            this.objectId = objectId;
        }

        /**
         * @return The objectType of this user.
         */
        public String getObjectType() {
            return objectType;
        }

        /**
         * @param objectType The objectType to set to this User object.
         */
        public void setObjectType(String objectType) {
            this.objectType = objectType;
        }

        /**
         * @return The userPrincipalName of this user.
         */
        public String getUserPrincipalName() {
            return userPrincipalName;
        }

        /**
         * @param userPrincipalName The userPrincipalName to set to this User object.
         */
        public void setUserPrincipalName(String userPrincipalName) {
            this.userPrincipalName = userPrincipalName;
        }

        /**
         * @return The usageLocation of this user.
         */
        public String getUsageLocation() {
            return usageLocation;
        }

        /**
         * @param usageLocation The usageLocation to set to this User object.
         */
        public void setUsageLocation(String usageLocation) {
            this.usageLocation = usageLocation;
        }

        /**
         * @return The telephoneNumber of this user.
         */
        public String getTelephoneNumber() {
            return telephoneNumber;
        }

        /**
         * @param telephoneNumber The telephoneNumber to set to this User object.
         */
        public void setTelephoneNumber(String telephoneNumber) {
            this.telephoneNumber = telephoneNumber;
        }

        /**
         * @return The surname of this user.
         */
        public String getSurname() {
            return surname;
        }

        /**
         * @param surname The surname to set to this User object.
         */
        public void setSurname(String surname) {
            this.surname = surname;
        }

        /**
         * @return The streetAddress of this user.
         */
        public String getStreetAddress() {
            return streetAddress;
        }

        /**
         * @param streetAddress The streetAddress to set to this user.
         */
        public void setStreetAddress(String streetAddress) {
            this.streetAddress = streetAddress;
        }

        /**
         * @return The state of this user.
         */
        public String getState() {
            return state;
        }

        /**
         * @param state The state to set to this User object.
         */
        public void setState(String state) {
            this.state = state;
        }

        /**
         * @return The preferredLanguage of this user.
         */
        public String getPreferredLanguage() {
            return preferredLanguage;
        }

        /**
         * @param preferredLanguage The preferredLanguage to set to this user.
         */
        public void setPreferredLanguage(String preferredLanguage) {
            this.preferredLanguage = preferredLanguage;
        }

        /**
         * @return The postalCode of this user.
         */
        public String getPostalCode() {
            return postalCode;
        }

        /**
         * @param postalCode The postalCode to set to this user.
         */
        public void setPostalCode(String postalCode) {
            this.postalCode = postalCode;
        }

        /**
         * @return The physicalDeliveryOfficeName of this user.
         */
        public String getPhysicalDeliveryOfficeName() {
            return physicalDeliveryOfficeName;
        }

        /**
         * @param physicalDeliveryOfficeName The physicalDeliveryOfficeName to set to this User object.
         */
        public void setPhysicalDeliveryOfficeName(String physicalDeliveryOfficeName) {
            this.physicalDeliveryOfficeName = physicalDeliveryOfficeName;
        }

        /**
         * @return The passwordPolicies of this user.
         */
        public String getPasswordPolicies() {
            return passwordPolicies;
        }

        /**
         * @param passwordPolicies The passwordPolicies to set to this User object.
         */
        public void setPasswordPolicies(String passwordPolicies) {
            this.passwordPolicies = passwordPolicies;
        }

        /**
         * @return The mobile of this user.
         */
        public String getMobile() {
            return mobile;
        }

        /**
         * @param mobile The mobile to set to this User object.
         */
        public void setMobile(String mobile) {
            this.mobile = mobile;
        }

        /**
         * @return The password of this user.
         */
        public String getPassword() {
            return password;
        }

        /**
         * @param password The mobile to set to this User object.
         */
        public void setPassword(String password) {
            this.password = password;
        }

        /**
         * @return The mail of this user.
         */
        public String getMail() {
            return mail;
        }

        /**
         * @param mail The mail to set to this User object.
         */
        public void setMail(String mail) {
            this.mail = mail;
        }

        /**
         * @return The MailNickname of this user.
         */
        public String getMailNickname() {
            return mailNickname;
        }

        /**
         * @param mail The MailNickname to set to this User object.
         */
        public void setMailNickname(String mailNickname) {
            this.mailNickname = mailNickname;
        }

        /**
         * @return The jobTitle of this user.
         */
        public String getJobTitle() {
            return jobTitle;
        }

        /**
         * @param jobTitle The jobTitle to set to this User object.
         */
        public void setJobTitle(String jobTitle) {
            this.jobTitle = jobTitle;
        }

        /**
         * @return The givenName of this user.
         */
        public String getGivenName() {
            return givenName;
        }

        /**
         * @param givenName The givenName to set to this User object.
         */
        public void setGivenName(String givenName) {
            this.givenName = givenName;
        }

        /**
         * @return The facsimileTelephoneNumber of this user.
         */
        public String getFacsimileTelephoneNumber() {
            return facsimileTelephoneNumber;
        }

        /**
         * @param facsimileTelephoneNumber The facsimileTelephoneNumber to set to this User object.
         */
        public void setFacsimileTelephoneNumber(String facsimileTelephoneNumber) {
            this.facsimileTelephoneNumber = facsimileTelephoneNumber;
        }

        /**
         * @return The displayName of this user.
         */
        public String getDisplayName() {
            return displayName;
        }

        /**
         * @param displayName The displayName to set to this User object.
         */
        public void setDisplayName(String displayName) {
            this.displayName = displayName;
        }

        /**
         * @return The dirSyncEnabled of this user.
         */
        public String getDirSyncEnabled() {
            return dirSyncEnabled;
        }

        /**
         * @param dirSyncEnabled The dirSyncEnabled to set to this User object.
         */
        public void setDirSyncEnabled(String dirSyncEnabled) {
            this.dirSyncEnabled = dirSyncEnabled;
        }

        /**
         * @return The department of this user.
         */
        public String getDepartment() {
            return department;
        }

        /**
         * @param department The department to set to this User object.
         */
        public void setDepartment(String department) {
            this.department = department;
        }

        /**
         * @return The lastDirSyncTime of this user.
         */
        public String getLastDirSyncTime() {
            return lastDirSyncTime;
        }

        /**
         * @param lastDirSyncTime The lastDirSyncTime to set to this User object.
         */
        public void setLastDirSyncTime(String lastDirSyncTime) {
            this.lastDirSyncTime = lastDirSyncTime;
        }

        /**
         * @return The country of this user.
         */
        public String getCountry() {
            return country;
        }

        /**
         * @param country The country to set to this user.
         */
        public void setCountry(String country) {
            this.country = country;
        }

        /**
         * @return The city of this user.
         */
        public String getCity() {
            return city;
        }

        /**
         * @param city The city to set to this user.
         */
        public void setCity(String city) {
            this.city = city;
        }

        /**
         * @return The accountEnabled attribute of this user.
         */
        public String getAccountEnabled() {
            return accountEnabled;
        }

        /**
         * @param accountEnabled The accountEnabled to set to this user.
         */
        public void setAccountEnabled(String accountEnabled) {
            this.accountEnabled = accountEnabled;
        }

        public boolean isIsDeleted() {
            return this.isDeleted;
        }

        public void setIsDeleted(boolean isDeleted) {
            this.isDeleted = isDeleted;
        }

        @Override
        public String toString() {
            return new JSONObject(this).toString();
        }

        public String getManagerDisplayname(){
            return managerDisplayname;
        }

        public void setManagerDisplayname(String managerDisplayname){
            this.managerDisplayname = managerDisplayname;
        }
    }

    /**
    * The DirectReports class holds the essential data for a single DirectReport entry. That is,
    * it holds the displayName and the objectId of the direct entry. It also provides the
    * access methods to set or get the displayName and the ObjectId of this entry.
    */
    //class DirectReport extends User{
    //
    //    private String displayName;
    //    private String objectId;
    //     
    //    /**
    //     * Two arguments Constructor for the DirectReport class.
    //     * @param displayName
    //     * @param objectId
    //     */
    //    public DirectReport(String displayName, String objectId){
    //        this.displayName = displayName;
    //        this.objectId = objectId;
    //    }
    //
    //    /**
    //     * @return The displayName of this direct report entry.
    //     */
    //    public String getDisplayName() {
    //        return displayName;
    //    }
    //
    //    
    //    /**
    //     *  @return The objectId of this direct report entry.
    //     */
    //    public String getObjectId() {
    //        return objectId;
    //    }
    //
    //}

    ```

## <a name="step-7-create-the-authentication-model-and-controller-files-for-basicfilter"></a><span data-ttu-id="da1d9-188">Krok 7: Vytvoření ověřování modelu a řadiče soubory (pro BasicFilter)</span><span class="sxs-lookup"><span data-stu-id="da1d9-188">Step 7: Create the authentication model and controller files (for BasicFilter)</span></span>
<span data-ttu-id="da1d9-189">Jsme na vědomí, že Java lze podrobné, ale téměř hotovi.</span><span class="sxs-lookup"><span data-stu-id="da1d9-189">We acknowledge that Java can be verbose, but you're almost done.</span></span> <span data-ttu-id="da1d9-190">Než napíšete BasicFilter servlet pro zpracování žádostí, budete muset některé další pomocné soubory, které musí ADAL4J zápisu.</span><span class="sxs-lookup"><span data-stu-id="da1d9-190">Before you write the BasicFilter servlet to handle the requests, you need to write some more helper files that the ADAL4J needs.</span></span>

1. <span data-ttu-id="da1d9-191">Vytvořte soubor s názvem AuthHelper.java, který vám poskytne metody sloužící k určení stavu přihlášeného uživatele.</span><span class="sxs-lookup"><span data-stu-id="da1d9-191">Create a file called AuthHelper.java, which will give you methods to use to determine the state of the signed-in user.</span></span> <span data-ttu-id="da1d9-192">Tyto metody zahrnují:</span><span class="sxs-lookup"><span data-stu-id="da1d9-192">The methods include:</span></span>

 * <span data-ttu-id="da1d9-193">**isAuthenticated()**: vrátí, zda je přihlášený uživatel.</span><span class="sxs-lookup"><span data-stu-id="da1d9-193">**isAuthenticated()**: Returns whether the user is signed in.</span></span>
 * <span data-ttu-id="da1d9-194">**containsAuthenticationData()**: vrátí, zda je token obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="da1d9-194">**containsAuthenticationData()**: Returns whether the token has data.</span></span>
 * <span data-ttu-id="da1d9-195">**isAuthenticationSuccessful()**: vrátí, zda byla ověření úspěšná pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="da1d9-195">**isAuthenticationSuccessful()**: Returns whether the authentication was successful for the user.</span></span>

 <span data-ttu-id="da1d9-196">Pokud chcete vytvořit soubor AuthHelper.java, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-196">To create the AuthHelper.java file, paste the following code:</span></span>

    ```Java
    package com.microsoft.aad.adal4jsample;

    import java.util.Map;

    import javax.servlet.http.HttpServletRequest;

    import com.microsoft.aad.adal4j.AuthenticationResult;
    import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
    import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
    import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

    public final class AuthHelper {

        public static final String PRINCIPAL_SESSION_NAME = "principal";

        private AuthHelper() {
        }

        public static boolean isAuthenticated(HttpServletRequest request) {
            return request.getSession().getAttribute(PRINCIPAL_SESSION_NAME) != null;
        }

        public static AuthenticationResult getAuthSessionObject(
                HttpServletRequest request) {
            return (AuthenticationResult) request.getSession().getAttribute(
                    PRINCIPAL_SESSION_NAME);
        }

        public static boolean containsAuthenticationData(
                HttpServletRequest httpRequest) {
            Map<String, String[]> map = httpRequest.getParameterMap();
            return httpRequest.getMethod().equalsIgnoreCase("POST") && (httpRequest.getParameterMap().containsKey(
                            AuthParameterNames.ERROR)
                            || httpRequest.getParameterMap().containsKey(
                                    AuthParameterNames.ID_TOKEN) || httpRequest
                            .getParameterMap().containsKey(AuthParameterNames.CODE));
        }

        public static boolean isAuthenticationSuccessful(
                AuthenticationResponse authResponse) {
            return authResponse instanceof AuthenticationSuccessResponse;
        }
    }
    ```

2. <span data-ttu-id="da1d9-197">Vytvořte soubor s názvem AuthParameterNames.java, který vám dává některé neměnné proměnné, které ADAL4J vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="da1d9-197">Create a file called AuthParameterNames.java, which gives you some immutable variables that the ADAL4J requires.</span></span> <span data-ttu-id="da1d9-198">Pokud chcete vytvořit soubor, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-198">To create the file, paste the following code:</span></span>

    ```Java
    package com.microsoft.aad.adal4jsample;

    public final class AuthParameterNames {

        private AuthParameterNames() {
        }

        public static String ERROR = "error";
        public static String ERROR_DESCRIPTION = "error_description";
        public static String ERROR_URI = "error_uri";
        public static String ID_TOKEN = "id_token";
        public static String CODE = "code";
    }
    ```

3. <span data-ttu-id="da1d9-199">Vytvořte soubor s názvem AadController.java, což je řadič vaší vzor MVC.</span><span class="sxs-lookup"><span data-stu-id="da1d9-199">Create a file called AadController.java, which is the controller of your MVC pattern.</span></span> <span data-ttu-id="da1d9-200">Soubor vám dává řadičem JSP a zpřístupní koncový bod adresy URL zabezpečení nebo aad pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="da1d9-200">The file gives you the JSP controller and exposes the secure/aad URL endpoint for the app.</span></span> <span data-ttu-id="da1d9-201">Soubor také obsahuje graf dotazu.</span><span class="sxs-lookup"><span data-stu-id="da1d9-201">The file also includes the graph query.</span></span> <span data-ttu-id="da1d9-202">Pokud chcete vytvořit soubor, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-202">To create the file, paste the following code:</span></span>

    ```Java
    package com.microsoft.aad.adal4jsample;

    import java.net.HttpURLConnection;
    import java.net.URL;

    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpSession;

    import org.json.JSONArray;
    import org.json.JSONObject;
    import org.springframework.stereotype.Controller;
    import org.springframework.ui.ModelMap;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;

    import com.microsoft.aad.adal4j.AuthenticationResult;

    @Controller
    @RequestMapping("/secure/aad")
    public class AadController {

        @RequestMapping(method = { RequestMethod.GET, RequestMethod.POST })
        public String getDirectoryObjects(ModelMap model, HttpServletRequest httpRequest) {
            HttpSession session = httpRequest.getSession();
            AuthenticationResult result = (AuthenticationResult) session.getAttribute(AuthHelper.PRINCIPAL_SESSION_NAME);
            if (result == null) {
                model.addAttribute("error", new Exception("AuthenticationResult not found in session."));
                return "/error";
            } else {
                String data;
                try {
                    data = this.getUsernamesFromGraph(result.getAccessToken(), session.getServletContext()
                            .getInitParameter("tenant"));
                    model.addAttribute("users", data);
                } catch (Exception e) {
                    model.addAttribute("error", e);
                    return "/error";
                }
            }
            return "/secure/aad";
        }

        private String getUsernamesFromGraph(String accessToken, String tenant) throws Exception {
            URL url = new URL(String.format("https://graph.windows.net/%s/users?api-version=2013-04-05", tenant,
                    accessToken));

            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            // Set the appropriate header fields in the request header.
            conn.setRequestProperty("api-version", "2013-04-05");
            conn.setRequestProperty("Authorization", accessToken);
            conn.setRequestProperty("Accept", "application/json;odata=minimalmetadata");
            String goodRespStr = HttpClientHelper.getResponseStringFromConn(conn, true);
            // logger.info("goodRespStr ->" + goodRespStr);
            int responseCode = conn.getResponseCode();
            JSONObject response = HttpClientHelper.processGoodRespStr(responseCode, goodRespStr);
            JSONArray users = new JSONArray();

            users = JSONHelper.fetchDirectoryObjectJSONArray(response);

            StringBuilder builder = new StringBuilder();
            User user = null;
            for (int i = 0; i < users.length(); i++) {
                JSONObject thisUserJSONObject = users.optJSONObject(i);
                user = new User();
                JSONHelper.convertJSONObjectToDirectoryObject(thisUserJSONObject, user);
                builder.append(user.getUserPrincipalName() + "<br/>");
            }
            return builder.toString();
        }

    }

    ```

## <a name="step-8-create-the-basicfilter-file-for-basicfilter-mvc"></a><span data-ttu-id="da1d9-203">Krok 8: Vytvoření souboru BasicFilter (pro BasicFilter MVC)</span><span class="sxs-lookup"><span data-stu-id="da1d9-203">Step 8: Create the BasicFilter file (for BasicFilter MVC)</span></span>
<span data-ttu-id="da1d9-204">Nyní můžete vytvořit soubor BasicFilter.java, která zpracovává požadavky z JSP zobrazit soubory.</span><span class="sxs-lookup"><span data-stu-id="da1d9-204">You can now create the BasicFilter.java file, which handles the requests from the JSP View files.</span></span> <span data-ttu-id="da1d9-205">Pokud chcete vytvořit soubor, vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="da1d9-205">To create the file, paste the following code:</span></span>

```Java

package com.microsoft.aad.adal4jsample;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.URI;
import java.net.URLEncoder;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;
import com.microsoft.aad.adal4j.ClientCredential;
import com.nimbusds.oauth2.sdk.AuthorizationCode;
import com.nimbusds.openid.connect.sdk.AuthenticationErrorResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

public class BasicFilter implements Filter {

    private String clientId = "";
    private String clientSecret = "";
    private String tenant = "";
    private String authority;

    public void destroy() {

    }

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {

        if (request instanceof HttpServletRequest) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            try {

                String currentUri = request.getScheme()
                        + "://"
                        + request.getServerName()
                        + ("http".equals(request.getScheme())
                                && request.getServerPort() == 80
                                || "https".equals(request.getScheme())
                                && request.getServerPort() == 443 ? "" : ":"
                                + request.getServerPort())
                        + httpRequest.getRequestURI();
                String fullUrl = currentUri
                        + (httpRequest.getQueryString() != null ? "?"
                                + httpRequest.getQueryString() : "");
                // check if user has a session
                if (!AuthHelper.isAuthenticated(httpRequest)) {
                    if (AuthHelper.containsAuthenticationData(httpRequest)) {
                        Map<String, String> params = new HashMap<String, String>();
                        for (String key : request.getParameterMap().keySet()) {
                            params.put(key,
                                    request.getParameterMap().get(key)[0]);
                        }
                        AuthenticationResponse authResponse = AuthenticationResponseParser
                                .parse(new URI(fullUrl), params);
                        if (AuthHelper.isAuthenticationSuccessful(authResponse)) {

                            AuthenticationSuccessResponse oidcResponse = (AuthenticationSuccessResponse) authResponse;
                            AuthenticationResult result = getAccessToken(
                                    oidcResponse.getAuthorizationCode(),
                                    currentUri);
                            createSessionPrincipal(httpRequest, result);
                        } else {
                            AuthenticationErrorResponse oidcResponse = (AuthenticationErrorResponse) authResponse;
                            throw new Exception(String.format(
                                    "Request for auth code failed: %s - %s",
                                    oidcResponse.getErrorObject().getCode(),
                                    oidcResponse.getErrorObject()
                                            .getDescription()));
                        }
                    } else {
                            // not authenticated
                            httpResponse.setStatus(302);
                            httpResponse
                                    .sendRedirect(getRedirectUrl(currentUri));
                            return;
                    }
                } else {
                    // if authenticated, how to check for valid session?
                    AuthenticationResult result = AuthHelper
                            .getAuthSessionObject(httpRequest);

                    if (httpRequest.getParameter("refresh") != null) {
                        result = getAccessTokenFromRefreshToken(
                                result.getRefreshToken(), currentUri);
                    } else {
                        if (httpRequest.getParameter("cc") != null) {
                            result = getAccessTokenFromClientCredentials();
                        } else {
                            if (result.getExpiresOnDate().before(new Date())) {
                                result = getAccessTokenFromRefreshToken(
                                        result.getRefreshToken(), currentUri);
                            }
                        }
                    }
                    createSessionPrincipal(httpRequest, result);
                }
            } catch (Throwable exc) {
                httpResponse.setStatus(500);
                request.setAttribute("error", exc.getMessage());
                httpResponse.sendRedirect(((HttpServletRequest) request)
                        .getContextPath() + "/error.jsp");
            }
        }
        chain.doFilter(request, response);
    }

    private AuthenticationResult getAccessTokenFromClientCredentials()
            throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", new ClientCredential(clientId,
                            clientSecret), null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private AuthenticationResult getAccessTokenFromRefreshToken(
            String refreshToken, String currentUri) throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByRefreshToken(refreshToken,
                            new ClientCredential(clientId, clientSecret), null,
                            null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;

    }

    private AuthenticationResult getAccessToken(
            AuthorizationCode authorizationCode, String currentUri)
            throws Throwable {
        String authCode = authorizationCode.getValue();
        ClientCredential credential = new ClientCredential(clientId,
                clientSecret);
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByAuthorizationCode(authCode, new URI(
                            currentUri), credential, null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private void createSessionPrincipal(HttpServletRequest httpRequest,
            AuthenticationResult result) throws Exception {
        httpRequest.getSession().setAttribute(
                AuthHelper.PRINCIPAL_SESSION_NAME, result);
    }

    private String getRedirectUrl(String currentUri)
            throws UnsupportedEncodingException {
        String redirectUrl = authority
                + this.tenant
                + "/oauth2/authorize?response_type=code%20id_token&scope=openid&response_mode=form_post&redirect_uri="
                + URLEncoder.encode(currentUri, "UTF-8") + "&client_id="
                + clientId + "&resource=https%3a%2f%2fgraph.windows.net"
                + "&nonce=" + UUID.randomUUID() + "&site_id=500879";
        return redirectUrl;
    }

    public void init(FilterConfig config) throws ServletException {
        clientId = config.getInitParameter("client_id");
        authority = config.getServletContext().getInitParameter("authority");
        tenant = config.getServletContext().getInitParameter("tenant");
        clientSecret = config.getInitParameter("secret_key");
    }

}

```

<span data-ttu-id="da1d9-206">Tato servlet zpřístupní všechny metody, které ADAL4J se očekávají z aplikace spustit.</span><span class="sxs-lookup"><span data-stu-id="da1d9-206">This servlet exposes all the methods that the ADAL4J will expect from the app to run.</span></span> <span data-ttu-id="da1d9-207">Tyto metody zahrnují:</span><span class="sxs-lookup"><span data-stu-id="da1d9-207">The methods include:</span></span>

* <span data-ttu-id="da1d9-208">**getAccessTokenFromClientCredentials()**: získá přístupový token z tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="da1d9-208">**getAccessTokenFromClientCredentials()**: Gets the access token from the secret.</span></span>
* <span data-ttu-id="da1d9-209">**getAccessTokenFromRefreshToken()**: získá přístupový token z tokenu obnovení.</span><span class="sxs-lookup"><span data-stu-id="da1d9-209">**getAccessTokenFromRefreshToken()**: Gets the access token from a refresh token.</span></span>
* <span data-ttu-id="da1d9-210">**getAccessToken()**: získá přístupový token z tok OpenID Connect (který používáte).</span><span class="sxs-lookup"><span data-stu-id="da1d9-210">**getAccessToken()**: Gets the access token from an OpenID Connect flow (which you use).</span></span>
* <span data-ttu-id="da1d9-211">**createSessionPrincipal()**: vytvoří objekt relace používat pro přístup k rozhraní Graph API.</span><span class="sxs-lookup"><span data-stu-id="da1d9-211">**createSessionPrincipal()**: Creates a session principal to use for Graph API access.</span></span>
* <span data-ttu-id="da1d9-212">**getRedirectUrl()**: získá redirectURL k porovnání s hodnotou zadanou v portálu.</span><span class="sxs-lookup"><span data-stu-id="da1d9-212">**getRedirectUrl()**: Gets the redirectURL to compare it with the value you entered in the portal.</span></span>

## <a name="step-9-compile-and-run-the-sample-in-tomcat"></a><span data-ttu-id="da1d9-213">Krok 9: Zkompilování a spuštění vzorového v Tomcat</span><span class="sxs-lookup"><span data-stu-id="da1d9-213">Step 9: Compile and run the sample in Tomcat</span></span>

1. <span data-ttu-id="da1d9-214">Změnit na kořenový adresář.</span><span class="sxs-lookup"><span data-stu-id="da1d9-214">Change to your root directory.</span></span>
2. <span data-ttu-id="da1d9-215">Chcete-li sestavit ukázku právě vložíte společně s použitím `maven`, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="da1d9-215">To build the sample you just put together by using `maven`, run the following command:</span></span>

    `$ mvn package`

 <span data-ttu-id="da1d9-216">Tento příkaz používá soubor pom.xml, který jste napsali závislosti.</span><span class="sxs-lookup"><span data-stu-id="da1d9-216">This command uses the pom.xml file that you wrote for dependencies.</span></span>

<span data-ttu-id="da1d9-217">Teď byste měli mít soubor adal4jsample.war ve vašem adresáři /targets.</span><span class="sxs-lookup"><span data-stu-id="da1d9-217">You should now have a adal4jsample.war file in your /targets directory.</span></span> <span data-ttu-id="da1d9-218">Můžete nasadit na soubor v kontejneru vaší Tomcat a najdete adrese http://localhost: 8080/adal4jsample nebo adresa URL.</span><span class="sxs-lookup"><span data-stu-id="da1d9-218">You can deploy the file in your Tomcat container and visit the http://localhost:8080/adal4jsample/ URL.</span></span>

> [!NOTE]
> <span data-ttu-id="da1d9-219">Můžete snadno nasadit .war soubor s nejnovější serverů Tomcat.</span><span class="sxs-lookup"><span data-stu-id="da1d9-219">You can easily deploy a .war file with the latest Tomcat servers.</span></span> <span data-ttu-id="da1d9-220">Přejděte na adrese http://localhost: 8080/manager/a postupujte podle pokynů pro nahrání souboru adal4jsample.war.</span><span class="sxs-lookup"><span data-stu-id="da1d9-220">Go to http://localhost:8080/manager/, and follow the instructions for uploading the adal4jsample.war file.</span></span> <span data-ttu-id="da1d9-221">Autodeploy ho bude pro vás správný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="da1d9-221">It will autodeploy for you with the correct endpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="da1d9-222">Další kroky</span><span class="sxs-lookup"><span data-stu-id="da1d9-222">Next steps</span></span>
<span data-ttu-id="da1d9-223">Teď máte funkční aplikaci Java, která můžete ověřovat uživatele, bezpečně volání webových rozhraní API pomocí OAuth 2.0 a získat základní informace o uživatelích.</span><span class="sxs-lookup"><span data-stu-id="da1d9-223">You now have a working Java app that can authenticate users, securely call web APIs using OAuth 2.0, and get basic information about the users.</span></span> <span data-ttu-id="da1d9-224">Pokud již nejsou naplněny vašeho klienta s uživateli, teď je vhodná doba na to udělat.</span><span class="sxs-lookup"><span data-stu-id="da1d9-224">If you haven't already populated your tenant with users, now is a good time to do so.</span></span>

<span data-ttu-id="da1d9-225">Pro odkaz na další můžete získat je hotová ukázka (bez vašich hodnot nastavení) v některém ze dvou způsobů:</span><span class="sxs-lookup"><span data-stu-id="da1d9-225">For additional reference, you can get the completed sample (without your configuration values) in either of two ways:</span></span>

* <span data-ttu-id="da1d9-226">Ho stáhnout jako [soubor .zip](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="da1d9-226">Download it as a [.zip file](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip).</span></span>
* <span data-ttu-id="da1d9-227">Soubor z Githubu naklonujte tím, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="da1d9-227">Clone the file from GitHub by entering the following command:</span></span>

 ```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```
