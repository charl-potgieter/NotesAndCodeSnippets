# Static website development using Hugo

## Hugo-book theme

 - https://themes.gohugo.io/themes/hugo-book/
 - Appropriate for docs adn book-style site

<br>

## Publishing to github pages

 - Ensure that a base URL is specified in the hugo.toml configuration file
 - For a git pages project site the URL will be 
 `http(s)://<username>.github.io/<repository>`  (my experience is https rather than http?)  Refer documentation and other options here:
 https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#types-of-github-pages-sites
 - Follow instructions here carefully: https://gohugo.io/hosting-and-deployment/hosting-on-github/
 - In particular ensure that the yaml file (refer link above):
     - contains the correct branch name
     - contains the correct hugo version name in the exact same format as example at above link

<br>

## Mathjax support

 - If file <site_root>/layouts/partials/docs/header.html does not exist copy it over from <site_root>\themes\<theme-name>\layouts\partials\docs
 - Add below to above file
    ```
    <script>
    MathJax = {
    tex: {
        inlineMath: [['$', '$'], ['\(', '\)']]
    }
    };
    </script>
    <script id="MathJax-script" async
    src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
    </script>
    ```
 - **Note** after above I have had cases where Latex does still not render - I think it is a case that header.html was never called.   I have  similarly copied footer.html from theme folder as above and added above javascript - seems to have solved the issue.
 - Above is adapted from https://github.com/matcornic/hugo-theme-learn/issues/188

<br>

 ## HTML inside of markdown
  - This is not enabled by default in hugo.
  - Need to add below to hugo.toml in site root:
      ```
    [markup.goldmark.renderer]
        unsafe = true
    ```
 - Note that the file still needs to start with either hugo front matter or at minimum a hash to indicate that the file is markdown
 -  The file needs to have a .md extension
 - Above is useful for pandas table visualisation to render the html styler