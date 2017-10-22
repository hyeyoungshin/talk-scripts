# talk-scripts

## Miscellaneous Notes

Here is one way to convert markdown to pdf using Atom.  It's a hacky work-around, but I
haven't found a better way.

1. Open the md file in Atom.

2. In the Packages menu, select `Markdown Preview Plus`.  You should see the nicely
   rendered preview of your document in another buffer.
   
3. Right click on the preview and select `Save as html`.

4. When the html file shows up in the Atom buffer, delete the following from the 
   html source code:
   From line 18, column 14 (beginning with `.markdown-preview`---but do not delete 
   the `<style>` tag), all the way to line 219, column 2.  
   
   (This step is not absolutely necessary. However, you will get better results without 
   all the `.markdown-preview` lines because those lines result in a gray background 
   which isn't good for pdf conversion.)
   
5. Open up the html file you just created in your web browser by typing the following in
   the url field:
   
           file:///Users... (full path to html file) .../talk-scripts/script-linking-types.html
		   
6. If you're happy with the look of the webpage, then use your browser's File menu to 
   select `Print`, and then choose `Save as PDF file` (instead of sending the page to a printer).
   
I know this seems like a lot of steps, but I don't know a better way, and once you get used
to doing this, it's all pretty fast and painless.

