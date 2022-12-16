# Add Google Analytics to Astro Project with Partytown

Google Analytics is a free tool or service from Google that aims to display visitor statistics from a website. Google Analytics can be used to analyze the performance of a website based on statistical data obtained such as total website visitors, where users access our website, which pages users access frequently, what type of browser users use when accessing our website and so on.

In this article, we will learn about how to add Google Analytics to Astro Project with Partytown.

## Partytown

Based on an explanation from the official Partytown website [https://partytown.builder.io/](https://partytown.builder.io/).

> "Partytown is a lazy-loaded library to help relocate resource intensive scripts into a [web worker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API), and off of the [main thread](https://developer.mozilla.org/en-US/docs/Glossary/Main_thread). Its goal is to help speed up sites by dedicating the main thread to your code, and offloading third-party scripts to a web worker."

So this is related to performance so that the use of scripts from third parties such as Google Analytics does not slow down our website.

## Getting Started

I assume that you already have an Astro project, if not you can create one first. You can check the page [https://docs.astro.build/en/install/auto/](https://docs.astro.build/en/install/auto/) for the installation of the Astro project.

## Preparing Google Analytics

First, we will create an account for Google Analytics [https://analytics.google.com](https://analytics.google.com). You can fill in the account name according to your project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671181815825/MDKOREj_3.png align="center")

Then, we fill in the property name, reporting time zone and currency.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671182012090/JbbQZna4d.png align="center")

Then, fill in related to your business/project and fill in as needed.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671182143811/mUyJqRWT-.png align="center")

After that, we select the platform of our project, we select the web platform

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671182345086/1-tq_Wxd3.png align="center")

Fill in your website URL with your Astro project web URL and fill the stream name. Then, click on create stream button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671182420790/1SLpHlbUv.png align="center")

And we will get information like the image below. We will get a `MEASUREMENT ID` for us to use in our project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671182663216/J2l-C00fy.png align="center")

## Install the Astro integrations for Partytown

You can use the `astro-add` command line tool for the installation.

```plaintext
# Using NPM
npx astro add partytown

# Using Yarn
yarn astro add partytown

# Using PNPM
pnpm astro add partytown
```

Or you can use the manual installation

```plaintext
npm install @astrojs/partytown
```

Add the Partytown integration in `astro.config.mjs` with the following code/config options.

```javascript
import { defineConfig } from "astro/config";
import partytown from "@astrojs/partytown";

export default defineConfig({
  integrations: [
    partytown({
      config: {
        forward: ["dataLayer.push"],
      },
    }),
  ],
});
```

Google Tag Manager uses the `dataLayer` array to send events. In order for Partytown to forward the calls to `window.dataLayer.push({..})` . Therefore, we use

```javascript
{
    forward: ["dataLayer.push"],
}
```

for forwarding all events to Google Analytics. You can read this in the [Partytown documentation](https://partytown.builder.io/google-tag-manager#example-config).

## Integration

Now Partytown integration is installed and we also have `MEASUREMENT ID` from Google Analytics

Google Analytics provide script like the script below for us to use in our project

```xml
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

Modify the script to allow Partytown to run Google Analytics in a web worker.

1.  Replace the `GA_MEASUREMENT_ID` with your own `GA MEASUREMENT ID` or `TRACKING ID`
    
2.  Add the `type` attribute to `script` tag and set the value of the type to `text/partytown`
    

```xml
<!-- Google tag (gtag.js) -->
<script
    type="text/partytown"
    async
	src="https://www.googletagmanager.com/gtag/js?id=G-VQSL0VL20K"
></script>
		
<script type="text/partytown">
    window.dataLayer = window.dataLayer || [];

    function gtag() {
		window.dataLayer.push(arguments);
	}
	gtag("js", new Date());
	gtag("config", "G-VQSL0VL20K");
</script>
```

Congratulations! We have successfully added Google Analytics to our Astro project.

You can test it and check on your Google Analytics page did it works or not.

Read More:

[https://partytown.builder.io/](https://partytown.builder.io/)

[https://partytown.builder.io/google-tag-manager#example-config](https://partytown.builder.io/google-tag-manager#example-config)

[https://docs.astro.build/en/guides/integrations-guide/partytown/](https://docs.astro.build/en/guides/integrations-guide/partytown/)