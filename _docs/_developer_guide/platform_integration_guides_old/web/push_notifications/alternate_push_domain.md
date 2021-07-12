---
nav_title: Alternate Web Push Domain
platform: Web
page_order: 20

page_type: reference
description: "This article covers how to integrate Braze Web Push on an alternate domain."

---

# Alternate Web Push Domain

To integrate Push on the web, your domain must be [secure][2], which generally means `https`, `localhost`, and other exceptions as defined in the [W3C Push Standard][1]. You'll also need to be able to register a Service Worker at the root of your domain, or at least be able to control the HTTP headers for that file.

_If you aren't able to meet all of those criteria_, use this guide to set up a workaround that lets you add a Push Prompt dialog to your website. 

For example, if you want the user to opt-in from an `http` (insecure) website, or from a Browser Extension popup which prevent the Push Prompt from displaying, keep reading!

**Caveats**:
Keep in mind that like many workarounds on the web, browsers continually evolve and in the future this may not work as intended.

* This requires that you own a separate secure domain (`https://`) and have access to register a Service Worker on that domain.
* This requires users to be logged-in to your website, to ensure that push tokens are tied to the same profiles.

To make the example below more clear, we'll use use `http://insecure.com` and `https://secure.com` as our two domains with the goal of getting visitors to register for push on `http://insecure.com`. This example could also be applied to a `chrome-extension://` scheme for a browser extension's popup page.

## Step 1: Initiate Prompting Flow

On `insecure.com`, open a new window to your secure domain using a URL parameter to pass the currently logged-in user's Braze External ID.


**http://insecure.com**
```html
<button id="opt-in">Opt-In For Push</button>
<script>
// the same ID you would use with `appboy.changeUser`:
const user_id = getUserIdSomehow();
// pass the user ID into the secure domain URL:
const secure_url = `https://secure.com/push-registration.html?external_id=${user_id}`;

// when the user takes some action, open the secure URL in a new window
document.getElementById("opt-in").onclick = function(){
    if (!window.open(secure_url, 'Opt-In to Push', 'height=500,width=600,left=150,top=150')) {
        window.alert('The popup was blocked by your browser');
    } else {
        // user is shown a popup window
        // and you can now prompt for push in this window
    }
}
</script>
```

## Step 2: Register For Push

At this point, `secure.com` will open a popup window in which you can initialize the Braze Web SDK for the same User ID, and request the user's permission for Web Push.

**https://secure.com/push-registration.html**
```html
<html>
    <head>
        <title>Opt-In for Push</title>
        <script src="https://js.appboycdn.com/web-sdk/3.1/appboy.min.js"></script>
    </head>
    <body>
    <button id="opt-in">Opt In For Push</button>
    <script>
        // initialize Braze
        appboy.initialize("YOUR-API-KEY", {
            baseUrl: "YOUR-SDK-BASE-URL",
            enableLogging: true
        });
        // parse the `external_id` from the URL parameters
        const external_id = (location.search.substring(1).split('&').find(param => param.startsWith('external_id=')) || '').split('=')[1] || '';
        if (external_id) {
            appboy.changeUser(external_id);
        }
        appboy.openSession();
        appboy.display.automaticallyShowNewInAppMessages();

        // when the user click's our Opt In button, prompt for permission
        document.getElementById("opt-in").onclick = function(){
            appboy.registerAppboyPushMessages(() => {
                window.alert(`You are registered for push!`);
                window.close();
            }, () => {
                window.alert(`Something went wrong.`);
            });
        };
    </script>
    </body>
</html>
```

## Step 3: Communicate between domains (optional)

Now that users can opt-in from this workflow originating on `insecure.com`, you may want to modify your site based on if the user is already opted-in or not. There's no point in asking the user to register for push if they already are.

You can use iFrames and the [`postMessage`][3] API to communicate between your two domains. 

**insecure.com**

On our `insecure.com` domain, we will ask the secure domain (where push is _actually_ registered) for information on the current user's push registration:

```html
<!-- Create an iframe to the secure domain and run getPushStatus onload-->
<iframe id="push-status" src="https://secure.com/push-status.html" onload="getPushStatus()" style="display:none;"></iframe>

<script>
function getPushStatus(event){
    // send a message to the iframe asking for push status
    event.target.contentWindow.postMessage({type: 'get_push_status'}, 'https://secure.com');
    // listen for a response from the iframe's domain
    window.addEventListener("message", (event) => {
        if (event.origin === "http://insecure.com" && event.data.type === 'set_push_status') {
            // update the page based on the push permission we're told
            window.alert(`Is user registered for push? ${event.data.isPushPermissionGranted}`);
        }
    }   
}
</script>
```

**secure.com/push-status.html**

```html
<script src="https://js.appboycdn.com/web-sdk/3.1/appboy.min.js"></script>
<script>
// initialize Braze
appboy.initialize("YOUR-API-KEY", {
    baseUrl: "YOUR-SDK-BASE-URL",
    enableLogging: true
});

// listen for a request from our insecure page
window.addEventListener("message", (event) => {
    if (event.origin === "http://insecure.com") {
        // when they ask for push status, retrieve from Braze SDK
        if (event.data.type === 'get_push_status') {
            // send the parent window (insecure.com) the results
            window.top.postMessage({
                type: 'set_push_status',
                isPushPermissionGranted: appboy.isPushPermissionGranted()
            }, event.origin);
        }
    }
});
</script>
```



[1]: https://www.w3.org/TR/service-workers/#security-considerations
[2]: https://w3c.github.io/webappsec-secure-contexts/
[3]: https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage
