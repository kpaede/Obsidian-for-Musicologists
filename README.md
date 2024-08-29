# Obsidian for Musicologists: Integrating Music Notation, Academic Writing and Citation

# The Problem
As a musician or musicologist, I often face the following problem: I have to submit my academic papers in Microsoft Word. I even like the citing features of Word and Zotero combined – however, Word is not well-suited to the needs of musicians. Musical notation examples have to be embedded in Word as images, which means that any changes to a musical example require several steps: regenerating it in the notation software, exporting it, and then re-embedding it. Additionally, in Word, you can’t listen to the musical examples, change musical layout options, or display only a specific measure. You have to work with images and not with music. This is frustrating as long as the musical side is still a work-in-progress.
If you’ve found your way here, you probably know what solution I’m offering. Obsidian is a Markdown editor that is highly extensible, providing some practical features. By the end of this tutorial, you will have:

- A word processing tool that allows you to display notated music dynamically within Obsidian and edit it dynamically (externally for the content, internally for the display).
- A practical way to use musical symbols (or glyphs) within the your text, e.g. for musical analysis
- The ability to combine all of this with an academic reference management system (Zotero) that handles citations and footnotes effectively, displaying them dynamically as previews.
- And the ability to export all of this in a meaningful way

Obsidian could look like this after that: 
![Obsidian example](./Pasted%20image%2020240829115502.png)
#  Rendering Plugins: Musescore, lilypond, abc, Verovio
- We are going to use the [Verovio Music Renderer](https://github.com/kpaede/Verovio-Music-Renderer) I developed – but first let's check out other awesome solutions.
- For [LilyPond](https://github.com/fuzzbyte/obsidian-lilypond). LilyPond is an open standard for text-based music notation and produces very beautiful graphical results. The downside is that since the notation is text-based, it’s not exactly beginner-friendly. Still it's pretty useful to render LilyPond within Obsidian, if you use it.
- ABC is an open standard for music notation in text form. It serves as an input markup language in music software as well as a storage format for music files. With the [Obsidian ABC.JS plugin](https://github.com/abcjs-music/obsidian-plugin-abcjs), you can not only render and playback ABC files conveniently but also enter the ABC code directly into MuseScore into a code block. It is text-baed, but very beginner-friendly and perfect for small sketches.
- The [MuseScore integration plugin for Obsidian](https://github.com/RyotaUshio/obsidian-musescore#musescore-integration-plugin-for-obsidian) takes a different approach. It uses MuseScore's PDF rendering as an auto-export and is another very exciting approach to dynamic notation within Obsidian. Unlike the Verovio plugin (which we are going to use), it does not need the export step to MusicXML or MEI within MuseScore. Install it via [BRAT](https://github.com/TfTHacker/obsidian42-brat).  The creator of this plugin introduces it [here in the Obsidian Forum](https://forum.obsidian.md/t/musescore-sheet-music-embed-plugin/87084) in a very informative thread.

# Rendering, Playback and svg-Download of of MusicXML, MEI and abc with the Verovio-Plugin
Obsidian will never be a musical notation editor. Therefore, we will edit the sheet music in a specific notation editor and render it within Obsidian, where we can also adjust the rendering of this notation. For this, we use the [Verovio Music Renderer](https://github.com/kpaede/Verovio-Music-Renderer), a plugin I recently built due to the lack of alternatives for this specific case. This is a plugin for [Obsidian](https://obsidian.md/) that uses [Verovio](https://www.verovio.org/) – a lightweight open-source library for engraving Music Encoding Initiative (MEI) music scores (as well as ABC and MusicXML files) into SVG. With this plugin, you can render musical scores seamlessly within Obsidian, enhancing your efficiency when working with written music. This approach ensures that we ALWAYS think in MEI. Verovio converts everything internally into MEI. This is actually an advantage because MEI is the de facto standard in musicology.

The plugin currently has the following features:

- Rendering MEI, ABC, and MusicXML notation dynamically from files in the Obsidian folder (relative paths) and URLs (absolute paths).
- A download button for the rendered SVG file (the toolbar is visible when hovering the mouse over the rendered music).
- A settings menu to adjust various rendering options.
- Sound playback of the rendered music.
- Highlighting of live playback notes, synced to the sound playback (syncing is still not fully reliable, though).
- Opening the rendered file via an external editor (if you want to edit your files with one click – I recommend using [MuseScore](https://musescore.org/de).
- Rendering specific measure selections.
### Installation

This plugin is not yet part of the Obsidian community plugins. You can install it via [BRAT](https://github.com/TfTHacker/obsidian42-brat) (just add the URL you're on right now as a beta plugin). You can also do it manually: Copy the files main.js and manifest.json from the release (look right) into the plugin folder of your vault like this: VaultFolder/.obsidian/plugins/Verovio-Music-Renderer/.

Install the plugin, then copy the following into your Obsidian document:

````
COPY FROM HERE
```verovio
https://www.verovio.org/examples/downloads/Schubert_Lindenbaum.mei
```COPY UNTIL HERE

````

or just use a filename from a file in your Obsidian Vault like this:

````
COPY FROM HERE
```verovio
Schubert_Lindenbaum.mei
```COPY UNTIL HERE

````
### Rendering Setting
In the settings menu of the Obsidian plugin, you can adjust several important parameters globally for all renderings. You can also apply custom settings for a specific rendering by adding them to your code block in Obsidian. Please refer to the [Verovio documentation](https://book.verovio.org/toolkit-reference/toolkit-options.html) for available options. Note that not all options may work and that they interfere with each other.

````
COPY FROM HERE
```verovio
Schubert_Lindenbaum.mei
font: Leland
scale: 10
breaks: encoded
```COPY UNTIL HERE

````
### Rendering Specific Measures
A special feature of this plugin is rendering predefined measures. To render measures 1-10, you can use the measureRange command like in this example. Please note that in this example, measure 20 is not included in the rendering. The type of breaks you choose to render can greatly influence the output (or even make the plugin render nothing at all). For example, "encoded" breaks can result in a blank rendering if no encoded break exists in your selection. Because of this, "breaks: none" is added to the example below, which might be a good default option for rendering musical snippets. You can also use "start" and "end" instead of numbers, e.g. `measureRange: 15-end` – or just render single measures: `measureRange: 5`

````
COPY FROM HERE
```verovio
Schubert_Lindenbaum.mei
scale: 50
breaks: none
measureRange: 1-20
```COPY UNTIL HERE
````
### Opening with MuseScore
The "Open" button automatically opens the rendered file with the program associated with it in the operating system. MuseScore can import and export MEI as well as MusicXML files. **After you opened a file with MuseScore, you need to export it as MusicXML or MEI and overwrite the file you are dealing with.**


# Using musical Glyphs in Text
By default, Obsidian is not designed to use different fonts inline. However, with a few steps, you can achieve this:

- First, install the [Obsidian Custom Font Plugin](https://github.com/pourmand1376/obsidian-custom-font#obsidian-custom-font-plugin).
- Then, add the desired fonts within the plugin. I recommend the following musical fonts by Dan Kreider: [MusGlyphs](https://www.notationcentral.com/product/musglyphs/) and [MusAnalysis](https://www.notationcentral.com/product/musanalysis/).
- After adding the fonts, select "Multiple Fonts" under the "Font" settings menu.
- To use these fonts, we need to add an additional CSS snippet in the general appearance settings of Obsidian, like this. This also ensures, ligatures are enabled, which is necessary for using these fonts.
```code
 div.MusAnalysis {
    font-family: "MusAnalysis", sans-serif !important;
    font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    -webkit-font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    -moz-font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    font-variant-ligatures: common-ligatures discretionary-ligatures historical-ligatures contextual;
    line-height: 3; /* Erhöhen Sie diesen Wert bei Bedarf */
}

div.MusGlyphs {
    font-family: "MusGlyphs", sans-serif !important;
    font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    -webkit-font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    -moz-font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    font-variant-ligatures: common-ligatures discretionary-ligatures historical-ligatures contextual;
    line-height: 3; /* Erhöhen Sie diesen Wert bei Bedarf */
}
span.MusAnalysis {
    font-family: "MusAnalysis", sans-serif !important;
    font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    font-variant-ligatures: common-ligatures discretionary-ligatures historical-ligatures contextual;
    line-height: 1.5; /* Adjust this as needed */
}
}

span.MusGlyphs {
    font-family: "MusGlyphs", sans-serif !important;
    font-feature-settings: "liga" 1, "clig" 1, "dlig" 1, "hlig" 1;
    font-variant-ligatures: common-ligatures discretionary-ligatures historical-ligatures contextual;
    line-height: 1.5; /* Adjust this as needed */
}
```
Then, you can add musical symbols inline as follows (and similarly for other fonts):
```code
<span class="MusAnalysis">
    &8Dv  &3T  &5DD/ &4Sg  
</span>
```
or as a separate paragraph like this:
```code
<div class="MusAnalysis">
    &8Dv  &3T  &5DD/ &4Sg  
</span>
```
I highly recommend setting up these with the [Templater Plugin](https://github.com/SilentVoid13/Templater) as shortcuts so you don't have to type them out every time. That way, you can add a musical glyph with one simple shortcut.<span class="MusGlyphs">TC B3b T6# 5/4</span>

# Footnotes, Reference Management and Zotero
### Footnotes
Footnotes are always a bit annoying in Obsidian. They are not kept up to date in the live preview, and creating them manually can be disruptive. However, since the last Obsidian update, there have been some improvements, such as the mouseover preview. We will use two plugins to significantly improve footnotes.

- The [Obsidian Footnotes Plugin](https://github.com/akaalias/obsidian-footnotes#obsidian-footnotes-plugin) allows you to assign a shortcut for creating footnotes. The documentation for this plugin makes it clear why this is helpful:

> [!NOTE]
>   This hotkey lets you:
> - Insert a new footnote marker (e.g. `[^1]`) with auto-incremented index in your text
> 	- Adds the footnote detail (e.g. `[^1]:` ) at the bottom of your text
> 	- Places your cursor so you can fill in the details quickly
> - Jump from your footnote TO the footnote detail
> - Jump from your footnote detail BACK to the footnote


To ensure that the order of footnotes is up to date, we also assign the numbering/updating of footnotes to a shortcut. We do this with the [Obsidian Tidy Footnotes](https://github.com/charliecm/obsidian-tidy-footnotes?tab=readme-ov-file#obsidian-tidy-footnotes) plugin. However, it's important to note that for later export, the current order of the footnotes doesn't matter. Pandoc is smart enough to automatically sort footnotes correctly according to their order in the text, so nothing gets mixed up.

### Literature references live preview and Zotero
I'm not going to go into too much detail here, as there are plenty of tutorials on this topic. Personally, I have found the following workflow to be effective:

With the [Obsidian ZotLit](https://github.com/PKM-er/obsidian-zotlit#obsidian-zotlit) plugin, I maintain the connection to Zotero and my bibliography. Within Zotero, I have all my PDFs and ePubs, and I annotate them there as well. I then access the annotations and highlights of the literature selected in Zotero via the side panel. The installation is very well described in the [Full Documentation](https://obzt.aidenlx.top/).

One issue remains: converting citations in pandoc format live into the desired citation style. This can be done with the [Obsidian Pandoc Reference List](https://github.com/mgmeyers/obsidian-pandoc-reference-list#obsidian-pandoc-reference-list) plugin. I also use the associated side panel here, which automatically displays a bibliography in the desired style for each document.

### Exporting
If you've installed the previous plugin, you already have Pandoc installed as well. We need this for exporting, for example, to a Word document, which also works with Zotero. I recommend using the [Obsidian Enhancing Export](https://github.com/mokeyish/obsidian-enhancing-export#obsidian-enhancing-export-plugin) plugin for this.

In [this thread](https://forum.obsidian.md/t/exporting-with-citations/75428/3), user Feralflora describes what needs to be done to ensure that the citations remain dynamically reusable in Word. We need a filter from this link: [https://retorque.re/zotero-better-bibtex/exporting/pandoc/index.html#from-markdown-to-zotero-live-citations](https://retorque.re/zotero-better-bibtex/exporting/pandoc/index.html#from-markdown-to-zotero-live-citations).

> [!NOTE]
> 1. Download the `zotero.lua` filter from the link above.
> 2. Find your Pandoc **user data folder** using the `pandoc --version` command in your OS’s terminal / command prompt.
>     - In this Pandoc folder, create a “filters” subfolder, and put the downloaded `zotero.lua` file into this folder.
>     - If no such user data folder exists at path you found in the terminal, create it and then also the aformentioned “filters” folder.
> 3. Put the following argument in the extra arguments of your **command template for Word** (docx) exports in Enhancing Export’s settings:
>     
>     `--lua-filter zotero.lua`
>     
>     - Double-check that you selected the right command template: Edit command template → Choose template → Word (.docx) template, before making any changes!
>     - Make sure to remove the `--citeproc`, `--bibliography`, `--csl` arguments, if you had added these. The `zotero.lua` filter will handle all this instead.
> 4. If necessary, specify additional arguments for `zotero.lua`.
>     - For example, if you are citing items in a group library, you should specify that with the `library` property (see below).
>     - This can be done in the note’s [properties 9](https://help.obsidian.md/Editing+and+formatting/Properties), like so:


Now you should be ready to go!
