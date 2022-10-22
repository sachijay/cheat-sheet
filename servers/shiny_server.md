# Run a webserver for Shiny apps

Shiny is an R package that makes it easy to build interactive web apps straight from [R](https://www.r-project.org/). See the [link](https://shiny.rstudio.com/) for more details about Shiny and example apps. The instructions are extracted from [https://www.rstudio.com/products/shiny/download-server/](https://www.rstudio.com/products/shiny/download-server/) and assumes a Ubuntu 16.04 or later platform. For other distributions see the website.

1. Install the latest version of [R](https://www.r-project.org/) from [CRAN](https://cloud.r-project.org/).
```bash
sudo apt update -qq
sudo apt install --no-install-recommends -y software-properties-common dirmngr
sudo wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
sudo apt install --no-install-recommends -y r-base zlib1g-dev make g++
```

2. Install the Shiny R package.
```bash
sudo su - \
-c "R -e \"install.packages(c('httpuv', 'later', 'promises', 'shiny'), repos='https://cran.rstudio.com/')\""
```

3. Install Shiny server.
```bash
sudo apt install gdebi-core
wget https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.17.973-amd64.deb
sudo gdebi shiny-server-1.5.17.973-amd64.deb
```

4. Make sure that port 3838 (default) is enabled for TCP.
	- For Google cloud; see (https://cloud.google.com/vpc/docs/using-firewalls).
	- For Azure; see (https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal).
	
**The Shiny server should be working now.** To access the app, go to `<<server-ip>>:3838` where `<<server-ip>>` is the IP or the domain of the server. The port `3838` is the default port.

5. To add apps to the server, add the app directory to `/srv/shiny-server/`.

6. Delete the welcome page to see the list of apps.
```bash
sudo rm /srv/shiny-server/index.html
```

## Important notes

- Make sure the packages required for the app are installed.

- Make sure that the user `shiny` has access to an app folder. If the app cannot be run due to permission issues, change the owner of the folder. For example, assuming the app is named `app1`;
```bash
sudo chown -R shiny /srv/shiny-server/app1
```

- If the page displays **"An error has occurred"**, check the logs at `/var/log/shiny-server/`.

## Optional: Install `tidyverse`

If you wish to install the [`tidyverse`](https://www.tidyverse.org/) set of packages run the following commands.

```bash
sudo apt install -y libcurl4-openssl-dev libssl-dev libxml2-dev
sudo su - \
-c "R -e \"install.packages(c('tidyverse'), repos='https://cran.rstudio.com/')\""
```
