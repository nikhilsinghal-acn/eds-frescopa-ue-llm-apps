# Fréscopa on Universal Editor, for the LLM Apps proof

This repository is the site code only. It is a copy of [aem-showcase/frescopa](https://github.com/aem-showcase/frescopa). The chat tool is a separate app, created with Adobe's automatic action setup, and it is not part of this repository.

The card we will show in the chat is the store locator.

| | |
| --- | --- |
| Action name | Find a Fréscopa café |
| Code identifier | `find_cafe` |
| Card | The Store Locator block. It already has a headline, a search box and a results list. |
| Page to publish | `/find-a-cafe` |

A café finder is the smallest useful card on this site. The quiz and the product pages need the commerce content and a longer flow. This one is a single block and a question a person would actually ask a chat.

## What has to stay as it is

`paths.json` maps `/content/frescopa/`. In the AEM site wizard the site name has to be exactly `frescopa`. Any other name publishes to an empty site.

## One-time setup in GitHub

1. Install the [AEM Code Sync](https://github.com/apps/aem-code-sync) app on `nikhilsinghal-acn/eds-frescopa-ue-llm-apps`.
2. Leave this repository as the code repository. Do not point the Veyra tool at it.

## Create the site on the author instance

Fréscopa does not publish a content package. The pages, products and café list live on Adobe's demo author, not in git. This repository brings the blocks, including Store Locator. The site content is created on your author.

1. Download the site template [aem-sites-with-edge-delivery-services-template-0.2.0.zip](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases/download/0.2.0/aem-sites-with-edge-delivery-services-template-0.2.0.zip).
2. In the AEM Sites console, choose Create, then Site from template, then Import, and upload that zip.
3. Create the site with these values.
   - Site title: `Fréscopa`
   - Site name: `frescopa`
   - Project URL: `https://main--eds-frescopa-ue-llm-apps--nikhilsinghal-acn.aem.page`
4. Open Tools, Cloud Services, Edge Delivery Services Configuration for the new site. On the Authentication tab, copy the technical account.
5. In [AEM user admin](https://tools.aem.live/tools/user-admin/index.html), add your own user as an admin first, then add the technical account as a config admin.
6. In `config.json` in this repository, replace `aem.author` and `aem.publish` with this program's author and publish hostnames. The values there today are Adobe's public demo.
7. Open `/content/frescopa` in the Sites console, edit the home page in Universal Editor, and publish it to Preview. Confirm `https://main--eds-frescopa-ue-llm-apps--nikhilsinghal-acn.aem.page` loads.
8. Create a page named `find-a-cafe`. Add one Store Locator block. Set the headline to "Find a Fréscopa café", the search placeholder to "City or postcode", and the button to "Search". Publish it to Preview and then Live.

The page will not list real cafés until a locations list is authored. For this proof, an empty search state on a branded page is enough to show that Universal Editor content renders in the chat. The café entries can be added in the same editor afterwards.

## Let Adobe create the action

Do this in a new LLM Apps app, not the Veyra one.

1. Create the app and choose the option for Adobe to configure the action.
2. When it asks which repository to create, accept a new one. Do not select `eds-llm-apps-repo` or this site repository.
3. Use the name "Find a Fréscopa café" and the identifier `find_cafe` if the wizard asks.
4. Choose an Edge Delivery card.
5. Script URL: `https://main--eds-frescopa-ue-llm-apps--nikhilsinghal-acn.aem.live/scripts/aem-embed.js`
6. Card URL: `https://main--eds-frescopa-ue-llm-apps--nikhilsinghal-acn.aem.live/find-a-cafe`
7. Allowed domains, for resource, connect and redirect: `main--eds-frescopa-ue-llm-apps--nikhilsinghal-acn.aem.live`
8. Deploy to the stage workspace and add that stage URL as a custom connector.

The embed script is not in this repository yet. It is the same chat loader used by the Veyra card, and it has to be added here before the script URL above will load. That is the next code change, once you confirm the author site is created.

## Cross-origin access

The chat loads the page from inside its own frame. On the site's Edge Delivery headers configuration (the `headers` content mapped by `paths.json`), add:

`Access-Control-Allow-Origin: *`

Publish the headers, then request the card page and confirm the response carries that header.

## What this proves, and what it does not

Publishing from Universal Editor and seeing that page as the card proves the authoring path. It does not prove that a chat can read the website, because the `aem.live` host still tells crawlers to stay away.
