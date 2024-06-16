# Infinicraft Project
The Minecraft Infinicraft mod from my Blucubed video. Feel free to use this code as long as you credit me (Spiralio or Blucubed). If you want to re-release any part of this code, talk to me first.
Blucubed Discord Server (for answering setup questions or crediting): https://discord.gg/TveN5TTE7J

⚠️ This is neither polished nor accessible! You will probably need some understanding of coding to get this to work. 


## Basic Structure
This system works in three parts.

### Minecraft Mod (Fabric 1.20.4)
The mod has two items: infinicraft:infinite and infinicraft:infinicrafter. The first acts as a "universal item" and can take on any texture. Textures are stored in a 16x16 2D array of integers representing colors (in decimal form), with -1 representing transparency (see `items.json` for sample arrays). The infinicrafter block takes two items and attempt to find a recipe for them.

If a recipe is not stored, the mod writes it to `craftQueue.json` (see Client Code). The mod also detects which items in `items.json` are "custom" (that is, not a vanilla item/block) and marks them as `custom: true`. This prompts the client code to generate a texture and add it to `items.json`, which the mod then reads and loads.

### Server Code (Python)
A script runs in the background and handles generation of new crafting recipes, and generates new textures. When the script detects a change to `craftQueue.json` (from the mod), it attempts to query ChatGPT for a new item. The crafted item is then stored in `items.json`.

The server also iterates through `items.json` to find custom items (`custom: true`) without a `texture` value. The client then sends a `GET` request to the server to generate a texture.

## Setup

### Mod Setup
Download the `infinicraft-mod` and open it in an IDE -- I recommend IntelliJ. The setup is similar to the Fabric mod quickstart tutorial, but here are the rough steps:

1. Ensure the project is set to Java 17
2. Run build.gradle
3. Restart your IDE
4. Run the genSources gradle task

Now you should be able to run the Minecraft Client tasks and launch the mod. You could also build the mod, though I never did that for the video.

### Client Code Setup
Create a `infinicraft` folder inside of you Minecraft build's `config` folder. Download `infinicraft-client` and move its contents into the new `infinicraft` folder. If you have an OpenAI API key, create a file called `apiKeys.json` in the `infinicraft` folder you created and place in the api key under "openAiKey". With NodeJS installed, run `npm i` inside of the folder. Edit the script.js file to include your server IP if you have one. Then run `node script.js` and it will start listening for changes.
