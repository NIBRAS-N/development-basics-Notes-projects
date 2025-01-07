# Why we use doctype in html?
-  specify the version of HTML being used.
# Use of Head tag
- The <head> tag is critical for:
  - Defining the webpage's metadata.
  - Linking stylesheets and scripts.
  - Improving SEO and user experience.
  - Providing browser and search engines with essential information about the document.
# what is the best place for script? head tag or body tag? 
- In the <head> Tag:
  - Use if the script is critical for the initial rendering (e.g., analytics, configuration).
  - Ensure it doesn’t block rendering by adding async or defer attributes
- At the End of the <body> Tag (Recommended for Most Cases):
  - Allows the browser to load and render the HTML before running scripts, improving page load speed.
  - Ideal for non-critical or interactive features.
