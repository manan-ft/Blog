# ERPNext v15 PDF Generation Issues

## Issues:
1. PDF not showing Header
2. PDF not retaining formatting
3. PDF not rendering CSS changes

## Background
ERPNext uses `wkhtmltopdf` package for rendering PDF files. All PDF generation issues arise out of two main reasons
1. Incorrect version of `wkhtmltopdf`
2. Incorrect `host_name` parameter in `site_config.json`

## Fixes
1. **Correct Version of** `wkhtmltopdf` Make sure you have correct version of wkhtmltopdf installed on ERPNext instance. Check the version using command `wkhtmltopdf --version` it should be `0.12.6.1-2`
In order to install correct version use the below commands
```
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_amd64.deb && \
sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_amd64.deb && \
sudo cp /usr/local/bin/wkhtmlto* /usr/bin/ && \
sudo chmod a+x /usr/bin/wk*
sudo rm wk* && \
sudo apt --fix-broken install -y && \
sudo apt install fontconfig xvfb libfontconfig xfonts-base xfonts-75dpi libxrender1 -y && \
```
2. Make sure to have correct `host_name` parameter and value in the `site_config.json`
Having `https` or `http`  is **important**
`host_name`:`https://erp.your-domain.com`
