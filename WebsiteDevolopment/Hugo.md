# Static website development using Hugo

## Hugo-book theme

 - https://themes.gohugo.io/themes/hugo-book/
 - Appropriate for docs adn book-style site

<br>

## Publishing to github pages

 - Ensure that a base URL is specified in the hugo.toml configuration file
 - Follow instructions here carefully: https://gohugo.io/hosting-and-deployment/hosting-on-github/
 - In particular ensure that the yaml file (refer link above):
     - contains the correct branch name
     - contains the correct hugo version name in the exact same format as example at above link

<br>

## Mathjax support

 - If file <site_root>/layouts/partials/docs does not exist copy it over from <site_root>\themes\<theme-name>\layouts\partials\docs
 - Add below to <site_root>/layouts/partials/docs
    ```<script>
    MathJax = {
    tex: {
        inlineMath: [['$', '$'], ['\(', '\)']]
    }
    };
    </script>
    <script id="MathJax-script" async
    src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
    </script>
 - Above is adapted from https://github.com/matcornic/hugo-theme-learn/issues/188
