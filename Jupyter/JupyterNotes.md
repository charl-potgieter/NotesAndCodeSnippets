# Jupyter(Lab) Notes

## Exporting styled pandas dataframes to webpdf rather than pdf

State below is as at 16 May 2023

 - Exporting styled pandas dataframes (http://pandas.pydata.org/pandas-docs/stable/user_guide/style.html) is currently not supported by nbconvert --to pdf
 - nbonvert --to webpdf does work with proper setup https://github.com/jupyter/nbconvert/issues/1395
- pyppeteer needs to be installed (python-pyppeteer on arch linux)
- Chromium itself needs to be installed 
- Chromium automation library is also required (in addition to Chromium) as per nbconvert docs (this is not the full version of chromium).   Run pyppeteer-install (this does not need to be done as a sudo'er - seems to download files to home dir only)
 - alternatively may be better to run as below to ensure latest version is available: <br>
 `jupyter nbconvert --to webpdf --allow-chromium-download NotebookName.ipynb`
  - Running above may hang.  See issue raised here. https://github.com/jupyter/nbconvert/issues/1834 <br> Sandbox needs to be disabled with : <br> `echo 'c.WebPDFExporter.disable_sandbox = True' > ~/.jupyter/jupyter_nbconvert_config.py && touch ~/.jupyter/jupyter_lab_config.py && echo 'c.WebPDFExporter.disable_sandbox = True' >> ~/.jupyter/jupyter_lab_config.py`
   - It doesn't seem as if any special Latex software is required to export the latex formulas in above process


## Sundry

 - Exporting to HTML using nbconvert does render latex maths formulas properly - the html file just doesnt open properly in JupyterLab, open with a browser.