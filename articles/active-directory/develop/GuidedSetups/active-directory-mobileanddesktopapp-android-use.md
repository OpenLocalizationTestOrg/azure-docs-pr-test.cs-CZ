---
title: "Azure AD v2 Android začínáte - použijte | Microsoft Docs"
description: "Jak získat přístupový token a volání rozhraní API, které vyžadují přístupové tokeny z koncového bodu Azure Active Directory v2 nebo Microsoft Graph API aplikace pro Android"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 7963a07a2b9d529e89302f32e5ffd56c51687ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a><span data-ttu-id="2856d-103">Použití knihovny ověřování společnosti Microsoft (MSAL) k získání tokenu pro rozhraní Microsoft Graph API</span><span class="sxs-lookup"><span data-stu-id="2856d-103">Use the Microsoft Authentication Library (MSAL) to get a token for the Microsoft Graph API</span></span>

1.  <span data-ttu-id="2856d-104">Otevřete: `MainActivity` (v části `app`  >  `java`  >  `{domain}.{appname}`)</span><span class="sxs-lookup"><span data-stu-id="2856d-104">Open: `MainActivity` (under `app` > `java` > `{domain}.{appname}`)</span></span>
2.  <span data-ttu-id="2856d-105">Přidejte následující importy:</span><span class="sxs-lookup"><span data-stu-id="2856d-105">Add the following imports:</span></span>

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
<span data-ttu-id="2856d-106">Nahraďte `MainActivity` třídy s níže:</span><span class="sxs-lookup"><span data-stu-id="2856d-106">Replace the `MainActivity` class with below:</span></span>
</li>
</ol>

```java
public class MainActivity extends AppCompatActivity {

    final static String CLIENT_ID = "[Enter the application Id here]";
    final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
    final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

    /* UI & Debugging Variables */
    private static final String TAG = MainActivity.class.getSimpleName();
    Button callGraphButton;
    Button signOutButton;

    /* Azure AD Variables */
    private PublicClientApplication sampleApp;
    private AuthenticationResult authResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        callGraphButton = (Button) findViewById(R.id.callGraph);
        signOutButton = (Button) findViewById(R.id.clearCache);

        callGraphButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onCallGraphClicked();
            }
        });

        signOutButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onSignOutClicked();
            }
        });

  /* Configure your sample app and save state for this activity */
        sampleApp = null;
        if (sampleApp == null) {
            sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    CLIENT_ID);
        }

  /* Attempt to get a user and acquireTokenSilent
   * If this fails we do an interactive request
   */
        List<User> users = null;

        try {
            users = sampleApp.getUsers();

            if (users != null && users.size() == 1) {
          /* We have 1 user */

                sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
            } else {
          /* We have no user */

          /* Let's do an interactive request */
                sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
            }
        } catch (MsalClientException e) {
            Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

        } catch (IndexOutOfBoundsException e) {
            Log.d(TAG, "User at this position does not exist: " + e.toString());
        }

    }

//
// App callbacks for MSAL
// ======================
// getActivity() - returns activity so we can acquireToken within a callback
// getAuthSilentCallback() - callback defined to handle acquireTokenSilent() case
// getAuthInteractiveCallback() - callback defined to handle acquireToken() case
//

    public Activity getActivity() {
        return this;
    }

    /* Callback method for acquireTokenSilent calls 
     * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
     * else errors that we need to do an interactive request.
     */
    private AuthenticationCallback getAuthSilentCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call Graph now */
                Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
                authResult = authenticationResult;

            /* call graph */
                callGraphAPI();

            /* update the UI to post call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed to acquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
                } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled the authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }


    /* Callback used for interactive request.  If succeeds we use the access
         * token to call the Microsoft Graph. Does not check cache
         */
    private AuthenticationCallback getAuthInteractiveCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
                Log.d(TAG, "Successfully authenticated");
                Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
                authResult = authenticationResult;

            /* call Graph */
                callGraphAPI();

            /* update the UI to post call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed to acquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled the authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }

    /* Set the UI for successful token acquisition data */
    private void updateSuccessUI() {
        callGraphButton.setVisibility(View.INVISIBLE);
        signOutButton.setVisibility(View.VISIBLE);
        findViewById(R.id.welcome).setVisibility(View.VISIBLE);
        ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                authResult.getUser().getName());
        findViewById(R.id.graphData).setVisibility(View.VISIBLE);
    }

    /* Use MSAL to acquireToken for the end-user
     * Callback will call Graph api w/ access token & update UI
     */
    private void onCallGraphClicked() {
        sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
    }

    /* Handles the redirect from the System Browser */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
    }

}
```
<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="2856d-107">Další informace</span><span class="sxs-lookup"><span data-stu-id="2856d-107">More Information</span></span>
#### <a name="getting-a-user-token-interactive"></a><span data-ttu-id="2856d-108">Získávání interaktivní token uživatele</span><span class="sxs-lookup"><span data-stu-id="2856d-108">Getting a user token interactive</span></span>
<span data-ttu-id="2856d-109">Volání `AcquireTokenAsync` metoda výsledky v okně výzvy pro uživatele k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2856d-109">Calling the `AcquireTokenAsync` method results in a window prompting the user to sign in.</span></span> <span data-ttu-id="2856d-110">Aplikace obvykle vyžadují uživatel interaktivní přihlášení při prvním potřebují přístup k chráněnému prostředku nebo když tichou operaci získat token selže (například heslo uživatele jeho platnost).</span><span class="sxs-lookup"><span data-stu-id="2856d-110">Applications usually require a user to sign in interactively the first time they need to access a protected resource, or when a silent operation to acquire a token fails (e.g. the user’s password expired).</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="2856d-111">Získání tokenu uživatele bez upozornění</span><span class="sxs-lookup"><span data-stu-id="2856d-111">Getting a user token silently</span></span>
<span data-ttu-id="2856d-112">`AcquireTokenSilentAsync`zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="2856d-112">`AcquireTokenSilentAsync` handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="2856d-113">Po `AcquireTokenAsync` se spustí poprvé, `AcquireTokenSilentAsync` je metoda běžně používají k získání tokenů pro přístup k chráněným prostředkům pro následující volání - jako volání na vyžádání nebo obnovení tokeny jsou vytvářeny bezobslužně.</span><span class="sxs-lookup"><span data-stu-id="2856d-113">After `AcquireTokenAsync` is executed for the first time, `AcquireTokenSilentAsync` is the method commonly used to obtain tokens to access protected resources for subsequent calls - as calls to request or renew tokens are made silently.</span></span>
<span data-ttu-id="2856d-114">Nakonec `AcquireTokenSilentAsync` selže – např. uživatel má odhlášení, nebo došlo ke změně hesla na jiném zařízení.</span><span class="sxs-lookup"><span data-stu-id="2856d-114">Eventually, `AcquireTokenSilentAsync` will fail – e.g. the user has signed out, or has changed their password on another device.</span></span> <span data-ttu-id="2856d-115">Když MSAL zjistí, že problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MsalUiRequiredException`.</span><span class="sxs-lookup"><span data-stu-id="2856d-115">When MSAL detects that the issue can be resolved by requiring an interactive action, it fires an `MsalUiRequiredException`.</span></span> <span data-ttu-id="2856d-116">Aplikace může zpracovat výjimku dvěma způsoby:</span><span class="sxs-lookup"><span data-stu-id="2856d-116">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="2856d-117">Volání proti `AcquireTokenAsync` okamžitě, výsledkem výzvy k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2856d-117">Make a call against `AcquireTokenAsync` immediately, which results in prompting the user to sign-in.</span></span> <span data-ttu-id="2856d-118">Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline v aplikaci k dispozici obsah pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="2856d-118">This pattern is usually used in online applications where there is no offline content in the application available for the user.</span></span> <span data-ttu-id="2856d-119">Ukázka generované touto s průvodcem instalací používá tento vzor: Zobrazí se v čase akce první spustit ukázku: vzhledem k tomu, že žádný uživatel nikdy použili aplikaci, `PublicClientApp.Users.FirstOrDefault` bude obsahovat hodnotu null a `MsalUiRequiredException` dojde k výjimce.</span><span class="sxs-lookup"><span data-stu-id="2856d-119">The sample generated by this guided setup uses this pattern: you can see it in action the first time you execute the sample: because no user ever used the application, `PublicClientApp.Users.FirstOrDefault` will contain a null value, and an `MsalUiRequiredException` exception will be thrown.</span></span> <span data-ttu-id="2856d-120">Kód v ukázce pak ošetří výjimku voláním `AcquireTokenAsync` výsledkem výzvy k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2856d-120">The code in the sample then handles the exception by calling `AcquireTokenAsync` resulting in prompting the user to sign-in.</span></span>
2.  <span data-ttu-id="2856d-121">Aplikace můžete udělat taky vizuální označení uživateli, které interaktivní přihlášení je povinné, tak, aby si uživatel může vybrat správný čas pro přihlášení, nebo můžete zkusit aplikaci `AcquireTokenSilentAsync` později.</span><span class="sxs-lookup"><span data-stu-id="2856d-121">Applications can also make a visual indication to the user that an interactive sign-in is required, so the user can select the right time to sign in, or the application can retry `AcquireTokenSilentAsync` at a later time.</span></span> <span data-ttu-id="2856d-122">To se často používá, když je uživatel moci funkce aplikace bez narušení – například je offline obsah k dispozici v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2856d-122">This is commonly used when the user is able to access functionality of the application without being disrupted - for example, there is offline content available in the application.</span></span> <span data-ttu-id="2856d-123">V takovém případě se můžete rozhodnout uživatele, když chtějí přihlášení pro přístup k chráněnému prostředku nebo aktualizovat zastaralé informace, nebo aplikaci můžete rozhodnout opakujte `AcquireTokenSilentAsync` obnovení po síti poté, co byla dočasně nedostupná.</span><span class="sxs-lookup"><span data-stu-id="2856d-123">In this case, the user can decide when they want to sign in to access the protected resource, or to refresh the outdated information, or your application can decide to retry `AcquireTokenSilentAsync` when network is restored after being unavailable temporarily.</span></span>
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a><span data-ttu-id="2856d-124">Volání rozhraní Graph API Microsoft pomocí tokenu, který jste obdrželi</span><span class="sxs-lookup"><span data-stu-id="2856d-124">Call the Microsoft Graph API using the token you just obtained</span></span>
1.  <span data-ttu-id="2856d-125">Přidejte následující metody do `MainActivity` třídy:</span><span class="sxs-lookup"><span data-stu-id="2856d-125">Add the following methods into the `MainActivity` class:</span></span>

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="2856d-126">Další informace o tom, jak volání REST chráněné rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2856d-126">More information about making a REST call against a protected API</span></span>

<span data-ttu-id="2856d-127">V této ukázkové aplikaci `callGraphAPI` volání `getAccessToken` a potom provádí HTTP `GET` požadavek na prostředek, který vyžaduje token a vrátí obsah.</span><span class="sxs-lookup"><span data-stu-id="2856d-127">In this sample application, `callGraphAPI` calls `getAccessToken` and then makes an HTTP `GET` request against a resource that requires a token and returns the content.</span></span> <span data-ttu-id="2856d-128">Tato metoda přidá získal token v *HTTP autorizační hlavičky*.</span><span class="sxs-lookup"><span data-stu-id="2856d-128">This method adds the acquired token in the *HTTP Authorization header*.</span></span> <span data-ttu-id="2856d-129">Tato ukázka je prostředek Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele.</span><span class="sxs-lookup"><span data-stu-id="2856d-129">For this sample, the resource is the Microsoft Graph API *me* endpoint – which displays the user's profile information.</span></span>
<!--end-collapse-->

## <a name="setup-sign-out"></a><span data-ttu-id="2856d-130">Instalační program odhlášení</span><span class="sxs-lookup"><span data-stu-id="2856d-130">Setup Sign-out</span></span>

1.  <span data-ttu-id="2856d-131">Přidejte následující metody do `MainActivity` třídy:</span><span class="sxs-lookup"><span data-stu-id="2856d-131">Add the following methods into the `MainActivity` class:</span></span>

```java
/* Clears a user's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt to get a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set the UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="2856d-132">Další informace</span><span class="sxs-lookup"><span data-stu-id="2856d-132">More information</span></span>

<span data-ttu-id="2856d-133">`onSignOutClicked`výše odebere uživatele z mezipaměti uživatele MSAL – se efektivně tak dozví MSAL zapomenutí aktuálního uživatele, takže budoucí žádost o získání tokenu bude úspěšné pouze v případě, že je k interaktivní.</span><span class="sxs-lookup"><span data-stu-id="2856d-133">`onSignOutClicked` above removes the user from MSAL user cache – this will effectively tell MSAL to forget the current user so a future request to acquire a token will only succeed if it is made to be interactive.</span></span>
<span data-ttu-id="2856d-134">I když aplikace v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde více účtů může být přihlášeného ve stejnou dobu – příklad je e-mailové aplikace, které má uživatel více účtů.</span><span class="sxs-lookup"><span data-stu-id="2856d-134">Although the application in this sample supports a single user, MSAL supports scenarios where multiple accounts can be signed-in at the same time – an example is an email application where a user has multiple accounts.</span></span>
<!--end-collapse-->
