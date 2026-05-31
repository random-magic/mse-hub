# Creating a custom MSE Hub

Follow these instructions to set up your own MSE Hub.

## Step 1: Fork me

If you're reading this, you're probably already here, but in case you aren't, navigate to https://github.com/ebbit1q/mse-hub/. At the top, click the "Fork" button to start creating your own fork of the repo.

![Fork](https://github.com/magictheegg/mse-hub-readme/blob/main/fork.png?raw=true)

On the next page, **change the "Repository name" field** to `username.github.io`. Replace `username` with your GitHub username. For example, if your username is `octocat`, the repository name should be `octocat.github.io`. This is **critical** for making your repo actually deploy to GitHub sites. You can add a Description if you want. Keep "Copy the `main` branch only" checked. Once you've renamed the repository, click "Create fork".

![Fork part 2](https://github.com/magictheegg/mse-hub-readme/blob/main/fork-part-two.png?raw=true)

It will now open your forked repo, it's recommended to bookmark or save this page. Close this guide and continue to step 2 in the copy of the guide on your forked repo.

#### Ensure github pages is configured correctly

Go to the "Settings" tab of your repo and then select "Pages" from the left menu, [here](../../settings/pages). Ensure the setting "Source" in the "Build and Deployment" section is set to "GitHub Actions":

<img width="266" height="115" alt="image" src="https://github.com/user-attachments/assets/91008dad-6060-4bec-b019-f4b7222636f4" />

## Step 2: Installing the exporter

Visit your generated github.io site, it will be at `https://username.github.io`. Replacing `username` with your GitHub username. For example if your username is `octocat` your site will be at `https://octocat.github.io`.

Edit the url, adding `/exporter.zip` to the end. For example if your username is `octocat` go to `https://octocat/github.io/exporter.zip`. Visit it and when prompted download this file. (alternatively, it's also in the resources folder of the repo)

Unzip it and place it into the "data" folder of MSE. Ensure the folder structure remains the same, your data folder should have one additional folder named `magic-egg-allinone-exporter.mse-export-template`.

## Step 3: Exporting set files

Start MSE. Open a set you'd like to export, then click File => Export => HTML ... and select Egg's All-in-One. This will export all of your site files.

> :memo: **Note:** This exporter uses the "Title" and "Set code" entered in the "Set info" tab of your MSE set. If those aren't set, the exporter will exhibit strange behavior.

The options you can select are as follows:
- **Export images**: Defaults to "Yes". Only change this if for some reason you don't need images with your export.
- **Image type**: Defaults to "Png". Github Pages sites can only host 1 GB of data before they start to error out, so if you're doing large exports, consider exporting some or all sets as "Jpg".
- **Symbol rarity**: Defaults to "Rare". This is the rarity color that your symbol icons will export as.
- **Draft structure**: Defaults to "Play booster". If your set is following a different booster structure, set it here. This is editable with a custom JSON while building the site.
- **Formats**: This is a text field where you can enter any custom formats your set is a part of, which can be queried in your site's search.
- **V mana replacement**: If you're using a custom V mana symbol, insert it here.

Once each of these options is filled out, click OK and save the set file as `set_code.txt`. Replace `set_code` with your set code. This should match the "Set code" in your "Set info" tab. This will take a second as the application exports all your images, and the end result is two outputs:
- `set_code.txt`, which is irrelevant.
- `set_code-files`, a directory containing all the files necessary to publish your set onto your hub.

## Step 4: Upload your files to github

In your repo go to the "sets" folder, then in the top right click "add file" then "upload files", [here](../../upload/main/sets). Then drag and drop the `set_code-files` folders you created in step 2 in there. It's also allowed to upload a zip file of your files folder.

## Future MSE Set Hub Updates

To get updates to the scripts or resources, go to your repo on github (saved in step 1). There, above the list of files click the "Sync fork" button and selec to update it.

If it's indicated that the new change comes with a change to the exporter, you can go through step 2 again, make sure it overwrites the previous exporter.

---

# Active Features & Customization

If you want to get fancy with your Hub, this is where the real power lies. Most of these features can be triggered by adding special text to your card data or dropping files into specific folders.

## 1. Mastering the Search Engine

The search bar is smarter than it looks. It supports a robust syntax similar to Scryfall for filtering your sets.

### Boolean Logic & Grouping
*   **AND**: Just use a space or `+`. `t:creature c:w` finds White creatures.
*   **OR**: Use `or`. `t:instant or t:sorcery` finds both.
*   **NOT**: Use a minus sign `-`. `-t:land` finds non-lands.
*   **Grouping**: Use parentheses `(...)`. `t:creature (c:b or c:r)` finds Black or Red creatures.

### Numerical Stats
You can use `:`, `=`, `>`, `<`, `>=`, or `<=` with any number-based field:
*   **Mana Value**: `mv:3` or `cmc=3`.
*   **Power/Toughness**: `pow>5`, `tou<2`.
*   **Loyalty**: `loyalty:4`.

### Colors & Identity
*   **Color**: `c:wu` (White-Blue), `c:m` (Multicolored), `c:3` (Exactly 3 colors).
*   **Guilds/Shards**: `c:boros`, `id:esper`, `c:azorius`.
*   **Identity**: `id:rg` or `ci:rg`.

### Advanced Queries
*   **Oracle Text**: `o:"draw a card"`.
*   **Regex**: For the real pros, use slashes for regex oracle search. `o:/[0-9] damage/`.
*   **Lore**: `lore:Akroma` searches the card name AND the flavor text.
*   **Keywords**: `has:flying` or `kw:cycling`.
*   **Shortcuts**: `is:permanent`, `is:spell`, `is:commander`, `is:hybrid`.
*   **Artist/Flavor**: `a:"John Avon"`, `ft:destiny`.
*   **Godzilla/Alias**: `alias:Godzilla` or `godzilla:Biollante`.

## 2. Card Notes (`notes` field magic)

In your MSE set's JSON, every card has a `"notes"` field. Typing these commands (usually starting with `!`) lets you control how they render or sort.

### Site & Search Logic
*   **`!group [Name]`**: Pulls the card into a specific section in the visual preview.
*   **`!sort [Value]`**: Overrides alphabetical sorting. Use `!sort 01`, `!sort 02`, etc., to manually order cards within a group. (Default sorting is `zzz`).
*   **`!tag [Name]`**: Adds a tag searchable via `tag:Name`.
*   **`!tag<category> [Value]`**: Create custom categories. Adding `!tag<mechanic> cycling` means a user can find the card by searching `mechanic:cycling`.
*   **`cube:[Name]`**: Flags the card for a specific cube search (`cube:Name`).

### External Tools (Cockatrice)
*   **`!tokens [Name]<count>`**: Automatically attaches tokens in Cockatrice. Example: `!tokens Elf Warrior<3>;Map<1>`.
*   **`!related [Name]`**: Links another card as "Related."
*   **`!tapped`**: Tells Cockatrice the card enters the battlefield tapped.

## 3. Set-Specific Overrides (`[code]-files` folder)

Every set has a folder at `sets/[SETCODE]-files/`. Dropping files here toggles advanced features for just that set.

### Layout & Branding
*   **`bg.png`**: Automatically becomes the background image for the set's pages.
*   **`logo.png`**: The main banner for the visual preview page.
*   **`splash.md`**: Enables a beautiful Markdown "Splash" page.
    *   **Image Shortcut**: Use `%Card Name%` in your Markdown to embed that card's image automatically.
*   **`card-notes/[Card Name].md`**: Create a Markdown file named after a card, and it will render as "Designer Notes" at the bottom of that card's individual page.

### The Visual Preview Layout (`preview-order.json`)
This file is the "director" of your preview gallery. It's an array of objects:
*   **Standard Group**: `{ "cards": ["Group1"], "title": "Artifacts" }`
    *   The `title` key automatically generates a header and an **Anchor** (`a->`).
*   **Pulling from other sets**: `{ "set": "OTHER", "cards": ["!tag Tag"], "logo": true }`
    *   `logo: true` inserts that set's **Logo Banner** (`l->`).
*   **Content Injection**: `{ "html": "lore.md" }`
    *   Injects a Markdown/HTML **Addenda** (`h->`) directly into the card grid.

### Build Control
*   **`structure.json`**: Overrides the global booster distribution for this set's draft packs.
*   **Booster Mode**: Setting `"draft_structure": "cube"` in your set JSON forces all cards to "Special" rarity for drafting.
*   **`previewed.txt`**: "Spoiler Mode"—list names here, and if the set is in `unpreviewed: empty` mode, only these cards appear.
*   **`ignore.txt`**: Completely hides the set from the search engine and "All Sets" page.
*   **`image_name: position`**: If you don't use the standard `number_name.png` format, add this to your set JSON to look for images by the `"position"` field instead.

## 4. Site-Wide Configuration

### The "Custom" Tree & Mirroring
The **`custom/`** folder is your best friend. The generator mirrors this folder directly to the site root during build.
*   To override a generated file (like `resources/mana.css`), place your version at `custom/resources/mana.css`.
*   To add a site-wide favicon, place it at `custom/img/favicon.png`.

### Home Page Magic
*   **`lists/set-order.json`**: Groups your sets into categories (like "Standard" or "Cube") on the Home page.
*   **`resources/gradients.json`**: Want a new Home page theme? Add two hex codes here. The first entry in the list becomes the site's default background theme.
