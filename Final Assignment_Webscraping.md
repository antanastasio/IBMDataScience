<center>
    <img src="https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/Logos/organization_logo/organization_logo.png" width="300" alt="cognitiveclass.ai logo"  />
</center>


<h1>Extracting Stock Data Using a Web Scraping</h1>


Not all stock data is available via API in this assignment; you will use web-scraping to obtain financial data. You will be quizzed on your results.\
Using beautiful soup we will extract historical share data from a web-page.


<h2>Table of Contents</h2>
<div class="alert alert-block alert-info" style="margin-top: 20px">
    <ul>
        <li>Downloading the Webpage Using Requests Library</li>
        <li>Parsing Webpage HTML Using BeautifulSoup</li>
        <li>Extracting Data and Building DataFrame</li>
    </ul>
<p>
    Estimated Time Needed: <strong>30 min</strong></p>
</div>

<hr>



```python
#!pip install pandas==1.3.3
#!pip install requests==2.26.0
!mamba install bs4==4.10.0 -y
!mamba install html5lib==1.1 -y
!pip install lxml==4.6.4
#!pip install plotly==5.3.1
```

    
                      __    __    __    __
                     /  \  /  \  /  \  /  \
                    /    \/    \/    \/    \
    ███████████████/  /██/  /██/  /██/  /████████████████████████
                  /  / \   / \   / \   / \  \____
                 /  /   \_/   \_/   \_/   \    o \__,
                / _/                       \_____/  `
                |/
            ███╗   ███╗ █████╗ ███╗   ███╗██████╗  █████╗
            ████╗ ████║██╔══██╗████╗ ████║██╔══██╗██╔══██╗
            ██╔████╔██║███████║██╔████╔██║██████╔╝███████║
            ██║╚██╔╝██║██╔══██║██║╚██╔╝██║██╔══██╗██╔══██║
            ██║ ╚═╝ ██║██║  ██║██║ ╚═╝ ██║██████╔╝██║  ██║
            ╚═╝     ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═════╝ ╚═╝  ╚═╝
    
            mamba (0.15.3) supported by @QuantStack
    
            GitHub:  https://github.com/mamba-org/mamba
            Twitter: https://twitter.com/QuantStack
    
    █████████████████████████████████████████████████████████████
    
    
    Looking for: ['bs4==4.10.0']
    
    pkgs/r/noarch            [<=>                 ] (00m:00s) 
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [<=>                 ] (00m:00s) 
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [<=>                 ] (00m:00s) 
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [=>                ] (00m:00s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [=>                ] (00m:00s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/linux-64          [<=>                 ] (00m:00s) 
    pkgs/r/noarch            [=>                ] (00m:00s) 404 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [=>                ] (00m:00s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/linux-64          [=>                ] (00m:00s) 408 KB / ?? (1.31 MB/s)
    pkgs/r/noarch            [<=>                 ] (00m:00s) Finalizing...
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [=>                ] (00m:00s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/linux-64          [=>                ] (00m:00s) 408 KB / ?? (1.31 MB/s)
    pkgs/r/noarch            [<=>                 ] (00m:00s) Done
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [=>                ] (00m:00s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/linux-64          [=>                ] (00m:00s) 408 KB / ?? (1.31 MB/s)
    pkgs/r/noarch            [====================] (00m:00s) Done
    pkgs/main/linux-64       [=>                ] (00m:00s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [=>                ] (00m:00s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/linux-64          [=>                ] (00m:00s) 408 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [<=>               ] (00m:01s) 396 KB / ?? (1.27 MB/s)
    pkgs/r/linux-64          [=>                ] (00m:01s) 408 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [<=>             ] (00m:01s) 769 KB / ?? (381.52 KB/s)
    pkgs/r/linux-64          [=>                ] (00m:01s) 408 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [<=>             ] (00m:01s) 769 KB / ?? (381.52 KB/s)
    pkgs/r/linux-64          [<=>               ] (00m:01s) 408 KB / ?? (1.31 MB/s)
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [<=>             ] (00m:01s) 769 KB / ?? (381.52 KB/s)
    pkgs/r/linux-64          [<=>             ] (00m:01s) 764 KB / ?? (378.80 KB/s)
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [ <=>                ] (00m:01s) Finalizing...
    pkgs/r/linux-64          [<=>             ] (00m:01s) 764 KB / ?? (378.80 KB/s)
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/main/noarch         [ <=>                ] (00m:01s) Done
    pkgs/r/linux-64          [<=>             ] (00m:01s) 764 KB / ?? (378.80 KB/s)
    pkgs/main/noarch         [====================] (00m:01s) Done
    pkgs/main/linux-64       [=>                ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/r/linux-64          [<=>             ] (00m:01s) 764 KB / ?? (378.80 KB/s)
    pkgs/main/linux-64       [<=>               ] (00m:01s) 440 KB / ?? (1.40 MB/s)
    pkgs/r/linux-64          [<=>             ] (00m:01s) 764 KB / ?? (378.80 KB/s)
    pkgs/main/linux-64       [<=>             ] (00m:01s) 828 KB / ?? (400.87 KB/s)
    pkgs/r/linux-64          [<=>             ] (00m:01s) 764 KB / ?? (378.80 KB/s)
    pkgs/main/linux-64       [<=>             ] (00m:01s) 828 KB / ?? (400.87 KB/s)
    pkgs/r/linux-64          [ <=>                ] (00m:01s) Finalizing...
    pkgs/main/linux-64       [<=>             ] (00m:01s) 828 KB / ?? (400.87 KB/s)
    pkgs/r/linux-64          [ <=>                ] (00m:01s) Done
    pkgs/r/linux-64          [====================] (00m:01s) Done
    pkgs/main/linux-64       [<=>             ] (00m:01s) 828 KB / ?? (400.87 KB/s)
    pkgs/main/linux-64       [ <=>            ] (00m:01s) 828 KB / ?? (400.87 KB/s)
    pkgs/main/linux-64       [  <=>               ] (00m:01s) 3 MB / ?? (1.41 MB/s)
    pkgs/main/linux-64       [   <=>              ] (00m:02s) 3 MB / ?? (1.41 MB/s)
    pkgs/main/linux-64       [   <=>              ] (00m:02s) 4 MB / ?? (1.65 MB/s)
    pkgs/main/linux-64       [   <=>              ] (00m:02s) Finalizing...
    pkgs/main/linux-64       [   <=>              ] (00m:02s) Done
    pkgs/main/linux-64       [====================] (00m:02s) Done
    
    Pinned packages:
      - python 3.7.*
    
    
    Transaction
    
      Prefix: /home/jupyterlab/conda/envs/python
    
      Updating specs:
    
       - bs4==4.10.0
       - ca-certificates
       - certifi
       - openssl
    
    
      Package               Version  Build           Channel                  Size
    ────────────────────────────────────────────────────────────────────────────────
      Install:
    ────────────────────────────────────────────────────────────────────────────────
    
    [32m  + beautifulsoup4 [00m      4.10.0  pyh06a4308_0    pkgs/main/noarch        85 KB
    [32m  + bs4            [00m      4.10.0  hd3eb1b0_0      pkgs/main/noarch        10 KB
    [32m  + soupsieve      [00m       2.3.1  pyhd3eb1b0_0    pkgs/main/noarch        34 KB
    
      Change:
    ────────────────────────────────────────────────────────────────────────────────
    
    [31m  - certifi        [00m   2021.10.8  py37h89c1867_1  installed                    
    [32m  + certifi        [00m   2021.10.8  py37h06a4308_2  pkgs/main/linux-64     151 KB
    
      Upgrade:
    ────────────────────────────────────────────────────────────────────────────────
    
    [31m  - ca-certificates[00m   2021.10.8  ha878542_0      installed                    
    [32m  + ca-certificates[00m  2021.10.26  h06a4308_2      pkgs/main/linux-64     115 KB
    [31m  - openssl        [00m      1.1.1l  h7f98852_0      installed                    
    [32m  + openssl        [00m      1.1.1m  h7f8727e_0      pkgs/main/linux-64       3 MB
    
      Summary:
    
      Install: 3 packages
      Change: 1 packages
      Upgrade: 2 packages
    
      Total download: 3 MB
    
    ────────────────────────────────────────────────────────────────────────────────
    
    Downloading  [>                                        ] (00m:00s)    2.69 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [>                                        ] (00m:00s)    2.69 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [=>                                       ] (00m:00s)  707.10 KB/s
    Extracting   [>                                                      ] (--:--) 
    [2A[0KFinished ca-certificates                      (00m:00s)             115 KB    704 KB/s
    Downloading  [=>                                       ] (00m:00s)  707.10 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [=>                                       ] (00m:00s)  707.10 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [=>                                       ] (00m:00s)  707.10 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [=>                                       ] (00m:00s)  707.10 KB/s
    Extracting   [======>                                  ] (00m:00s)        1 / 6
    Downloading  [===>                                     ] (00m:00s)    1.52 MB/s
    Extracting   [======>                                  ] (00m:00s)        1 / 6
    [2A[0KFinished certifi                              (00m:00s)             151 KB    881 KB/s
    Downloading  [===>                                     ] (00m:00s)    1.52 MB/s
    Extracting   [======>                                  ] (00m:00s)        1 / 6
    Downloading  [===>                                     ] (00m:00s)    1.52 MB/s
    Extracting   [======>                                  ] (00m:00s)        1 / 6
    Downloading  [===>                                     ] (00m:00s)    1.52 MB/s
    Extracting   [======>                                  ] (00m:00s)        1 / 6
    Downloading  [====>                                    ] (00m:00s)    1.97 MB/s
    Extracting   [======>                                  ] (00m:00s)        1 / 6
    Downloading  [====>                                    ] (00m:00s)    1.97 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    [2A[0KFinished beautifulsoup4                       (00m:00s)              85 KB    484 KB/s
    Downloading  [====>                                    ] (00m:00s)    1.97 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [====>                                    ] (00m:00s)    2.10 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [====>                                    ] (00m:00s)    2.10 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [====>                                    ] (00m:00s)    2.10 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    [2A[0KFinished soupsieve                            (00m:00s)              34 KB    191 KB/s
    Downloading  [====>                                    ] (00m:00s)    2.10 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [====>                                    ] (00m:00s)    2.10 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    [2A[0KFinished bs4                                  (00m:00s)              10 KB     55 KB/s
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [=============>                           ] (00m:00s)        2 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [====================>                    ] (00m:00s)        3 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [====================>                    ] (00m:00s)        3 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [===========================>             ] (00m:00s)        4 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [===========================>             ] (00m:00s)        4 / 6
    Downloading  [=====>                                   ] (00m:00s)    2.12 MB/s
    Extracting   [==================================>      ] (00m:00s)        5 / 6
    Downloading  [=========================================] (00m:00s)   14.15 MB/s
    Extracting   [==================================>      ] (00m:00s)        5 / 6
    [2A[0KFinished openssl                              (00m:00s)               3 MB     12 MB/s
    Downloading  [=========================================] (00m:00s)   14.15 MB/s
    Extracting   [==================================>      ] (00m:00s)        5 / 6
    Downloading  [=========================================] (00m:00s)   14.15 MB/s
    Extracting   [==================================>      ] (00m:00s)        5 / 6
    Downloading  [=========================================] (00m:00s)   14.15 MB/s
    Extracting   [==================================>      ] (00m:00s)        5 / 6
    Downloading  [=========================================] (00m:00s)   14.15 MB/s
    Extracting   [=========================================] (00m:00s)        6 / 6
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done
    
                      __    __    __    __
                     /  \  /  \  /  \  /  \
                    /    \/    \/    \/    \
    ███████████████/  /██/  /██/  /██/  /████████████████████████
                  /  / \   / \   / \   / \  \____
                 /  /   \_/   \_/   \_/   \    o \__,
                / _/                       \_____/  `
                |/
            ███╗   ███╗ █████╗ ███╗   ███╗██████╗  █████╗
            ████╗ ████║██╔══██╗████╗ ████║██╔══██╗██╔══██╗
            ██╔████╔██║███████║██╔████╔██║██████╔╝███████║
            ██║╚██╔╝██║██╔══██║██║╚██╔╝██║██╔══██╗██╔══██║
            ██║ ╚═╝ ██║██║  ██║██║ ╚═╝ ██║██████╔╝██║  ██║
            ╚═╝     ╚═╝╚═╝  ╚═╝╚═╝     ╚═╝╚═════╝ ╚═╝  ╚═╝
    
            mamba (0.15.3) supported by @QuantStack
    
            GitHub:  https://github.com/mamba-org/mamba
            Twitter: https://twitter.com/QuantStack
    
    █████████████████████████████████████████████████████████████
    
    
    Looking for: ['html5lib==1.1']
    
    pkgs/main/linux-64       Using cache
    pkgs/main/noarch         Using cache
    pkgs/r/linux-64          Using cache
    pkgs/r/noarch            Using cache
    
    Pinned packages:
      - python 3.7.*
    
    
    Transaction
    
      Prefix: /home/jupyterlab/conda/envs/python
    
      Updating specs:
    
       - html5lib==1.1
       - ca-certificates
       - certifi
       - openssl
    
    
      Package         Version  Build         Channel                 Size
    ───────────────────────────────────────────────────────────────────────
      Install:
    ───────────────────────────────────────────────────────────────────────
    
    [32m  + html5lib    [00m      1.1  pyhd3eb1b0_0  pkgs/main/noarch       91 KB
    [32m  + webencodings[00m    0.5.1  py37_1        pkgs/main/linux-64     19 KB
    
      Summary:
    
      Install: 2 packages
    
      Total download: 110 KB
    
    ───────────────────────────────────────────────────────────────────────
    
    Downloading  [======>                                  ] (00m:00s)  122.40 KB/s
    Extracting   [>                                                      ] (--:--) 
    [2A[0KFinished webencodings                         (00m:00s)              19 KB    122 KB/s
    Downloading  [======>                                  ] (00m:00s)  122.40 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [======>                                  ] (00m:00s)  122.40 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [======>                                  ] (00m:00s)  122.40 KB/s
    Extracting   [>                                                      ] (--:--) 
    Downloading  [======>                                  ] (00m:00s)  122.40 KB/s
    Extracting   [====================>                    ] (00m:00s)        1 / 2
    Downloading  [=========================================] (00m:00s)  674.34 KB/s
    Extracting   [====================>                    ] (00m:00s)        1 / 2
    [2A[0KFinished html5lib                             (00m:00s)              91 KB    557 KB/s
    Downloading  [=========================================] (00m:00s)  674.34 KB/s
    Extracting   [====================>                    ] (00m:00s)        1 / 2
    Downloading  [=========================================] (00m:00s)  674.34 KB/s
    Extracting   [====================>                    ] (00m:00s)        1 / 2
    Downloading  [=========================================] (00m:00s)  674.34 KB/s
    Extracting   [====================>                    ] (00m:00s)        1 / 2
    Downloading  [=========================================] (00m:00s)  674.34 KB/s
    Extracting   [=========================================] (00m:00s)        2 / 2
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done
    Requirement already satisfied: lxml==4.6.4 in /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages (4.6.4)



```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
```

## Using Webscraping to Extract Stock Data Example


First we must use the `request` library to downlaod the webpage, and extract the text. We will extract Netflix stock data <https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/netflix_data_webpage.html>.



```python
url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/netflix_data_webpage.html"

data  = requests.get(url).text
```

Next we must parse the text into html using `beautiful_soup`



```python
soup = BeautifulSoup(data, 'html5lib')
```

Now we can turn the html table into a pandas dataframe



```python
netflix_data = pd.DataFrame(columns=["Date", "Open", "High", "Low", "Close", "Volume"])

# First we isolate the body of the table which contains all the information
# Then we loop through each row and find all the column values for each row
for row in soup.find("tbody").find_all('tr'):
    col = row.find_all("td")
    date = col[0].text
    Open = col[1].text
    high = col[2].text
    low = col[3].text
    close = col[4].text
    adj_close = col[5].text
    volume = col[6].text
    
    # Finally we append the data of each row to the table
    netflix_data = netflix_data.append({"Date":date, "Open":Open, "High":high, "Low":low, "Close":close, "Adj Close":adj_close, "Volume":volume}, ignore_index=True)    
```

We can now print out the dataframe



```python
netflix_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jun 01, 2021</td>
      <td>504.01</td>
      <td>536.13</td>
      <td>482.14</td>
      <td>528.21</td>
      <td>78,560,600</td>
      <td>528.21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>May 01, 2021</td>
      <td>512.65</td>
      <td>518.95</td>
      <td>478.54</td>
      <td>502.81</td>
      <td>66,927,600</td>
      <td>502.81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Apr 01, 2021</td>
      <td>529.93</td>
      <td>563.56</td>
      <td>499.00</td>
      <td>513.47</td>
      <td>111,573,300</td>
      <td>513.47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mar 01, 2021</td>
      <td>545.57</td>
      <td>556.99</td>
      <td>492.85</td>
      <td>521.66</td>
      <td>90,183,900</td>
      <td>521.66</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Feb 01, 2021</td>
      <td>536.79</td>
      <td>566.65</td>
      <td>518.28</td>
      <td>538.85</td>
      <td>61,902,300</td>
      <td>538.85</td>
    </tr>
  </tbody>
</table>
</div>



We can also use the pandas `read_html` function using the url



```python
read_html_pandas_data = pd.read_html(url)
```

Or we can convert the BeautifulSoup object to a string



```python
read_html_pandas_data = pd.read_html(str(soup))
```

Beacause there is only one table on the page, we just take the first table in the list returned



```python
netflix_dataframe = read_html_pandas_data[0]

netflix_dataframe.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close*</th>
      <th>Adj Close**</th>
      <th>Volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jun 01, 2021</td>
      <td>504.01</td>
      <td>536.13</td>
      <td>482.14</td>
      <td>528.21</td>
      <td>528.21</td>
      <td>78560600</td>
    </tr>
    <tr>
      <th>1</th>
      <td>May 01, 2021</td>
      <td>512.65</td>
      <td>518.95</td>
      <td>478.54</td>
      <td>502.81</td>
      <td>502.81</td>
      <td>66927600</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Apr 01, 2021</td>
      <td>529.93</td>
      <td>563.56</td>
      <td>499.00</td>
      <td>513.47</td>
      <td>513.47</td>
      <td>111573300</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mar 01, 2021</td>
      <td>545.57</td>
      <td>556.99</td>
      <td>492.85</td>
      <td>521.66</td>
      <td>521.66</td>
      <td>90183900</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Feb 01, 2021</td>
      <td>536.79</td>
      <td>566.65</td>
      <td>518.28</td>
      <td>538.85</td>
      <td>538.85</td>
      <td>61902300</td>
    </tr>
  </tbody>
</table>
</div>



## Using Webscraping to Extract Stock Data Exercise


Use the `requests` library to download the webpage <https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/amazon_data_webpage.html>. Save the text of the response as a variable named `html_data`.



```python
url = 'https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/amazon_data_webpage.html'

html_data = requests.get(url).text
```

Parse the html data using `beautiful_soup`.



```python
soup = BeautifulSoup(html_data, 'html5lib')
```

<b>Question 1</b> What is the content of the title attribute:



```python
soup.head()
```




    [<script>window.performance && window.performance.mark && window.performance.mark('PageStart');</script>,
     <meta charset="utf-8"/>,
     <title>Netflix, Inc. (NFLX) Stock Historical Prices &amp; Data - Yahoo Finance</title>,
     <meta content="NFLX, Netflix, Inc., NFLX historical prices, Netflix, Inc. historical prices, historical prices, stocks, quotes, finance" name="keywords"/>,
     <meta content="on" http-equiv="x-dns-prefetch-control"/>,
     <meta content="on" property="twitter:dnt"/>,
     <meta content="458584288257241" property="fb:app_id"/>,
     <meta content="#400090" name="theme-color"/>,
     <meta content="width=device-width, initial-scale=1" name="viewport"/>,
     <meta content="Discover historical prices for NFLX stock on Yahoo Finance. View daily, weekly or monthly format back to when Netflix, Inc. stock was issued." lang="en-US" name="description"/>,
     <meta content="guce.yahoo.com" name="oath:guce:consent-host"/>,
     <meta content="A9862C0E6E1BE95BCE0BF3D0298FD58B" name="msvalidate.01"/>,
     <link href="/manifest.json" rel="manifest"/>,
     <link href="//l.yimg.com" rel="dns-prefetch"/>,
     <link href="//s.yimg.com" rel="dns-prefetch"/>,
     <link href="//csc.beap.bc.yahoo.com" rel="dns-prefetch"/>,
     <link href="//geo.query.yahoo.com" rel="dns-prefetch"/>,
     <link href="//y.analytics.yahoo.com" rel="dns-prefetch"/>,
     <link href="//b.scorecardresearch.com" rel="dns-prefetch"/>,
     <link href="//iquery.finance.yahoo.com" rel="dns-prefetch"/>,
     <link href="//fc.yahoo.com" rel="dns-prefetch"/>,
     <link href="//video-api.yql.yahoo.com" rel="dns-prefetch"/>,
     <link href="//yrtas.btrll.com" rel="dns-prefetch"/>,
     <link href="//shim.btrll.com" rel="dns-prefetch"/>,
     <link href="//consent.cmp.oath.com" rel="dns-prefetch"/>,
     <link href="//geo.yahoo.com" rel="dns-prefetch"/>,
     <link crossorigin="anonymous" href="//l.yimg.com" rel="preconnect"/>,
     <link crossorigin="anonymous" href="//s.yimg.com" rel="preconnect"/>,
     <link href="//csc.beap.bc.yahoo.com" rel="preconnect"/>,
     <link href="//geo.query.yahoo.com" rel="preconnect"/>,
     <link href="//y.analytics.yahoo.com" rel="preconnect"/>,
     <link href="//b.scorecardresearch.com" rel="preconnect"/>,
     <link href="//iquery.finance.yahoo.com" rel="preconnect"/>,
     <link href="//fc.yahoo.com" rel="preconnect"/>,
     <link href="//video-api.yql.yahoo.com" rel="preconnect"/>,
     <link href="//yrtas.btrll.com" rel="preconnect"/>,
     <link href="//shim.btrll.com" rel="preconnect"/>,
     <link href="//consent.cmp.oath.com" rel="preconnect"/>,
     <link href="//geo.yahoo.com" rel="preconnect"/>,
     <link href="//ads.yahoo.com" rel="preconnect"/>,
     <link as="font" crossorigin="anonymous" href="https://s.yimg.com/uc/finance/dd-site/fonts/YahooSansFinancial-Regular-Web.woff2" rel="preload" type="font/woff2"/>,
     <link as="font" crossorigin="anonymous" href="https://s.yimg.com/uc/finance/dd-site/fonts/YahooSansFinancial-Medium-Web.woff2" rel="preload" type="font/woff2"/>,
     <link as="font" crossorigin="anonymous" href="https://s.yimg.com/uc/finance/dd-site/fonts/YahooSansFinancial-Semibold-Web.woff2" rel="preload" type="font/woff2"/>,
     <link as="font" crossorigin="anonymous" href="https://s.yimg.com/uc/finance/dd-site/fonts/YahooSansFinancial-Bold-Web.woff2" rel="preload" type="font/woff2"/>,
     <link as="worker" href="/__finStreamer-worker.js" rel="preload"/>,
     <link as="worker" href="/__rapidworker-1.2.js" rel="preload"/>,
     <link href="https://s.yimg.com/cv/apiv2/default/icons/favicon_y19_32x32_custom.svg" rel="icon" sizes="any"/>,
     <link href="https://s.yimg.com/cv/apiv2/default/fp/20180826/icons/favicon_y19_32x32.ico" rel="alternate icon" type="image/x-icon"/>,
     <link href="https://finance.yahoo.com/quote/NFLX/history/" rel="canonical"/>,
     <meta content="@YahooFinance" property="twitter:site"/>,
     <meta content="458584288257241" property="fb:pages"/>,
     <meta content="https://s.yimg.com/cv/apiv2/social/images/yahoo_default_logo.png" property="og:image"/>,
     <meta content="Discover historical prices for NFLX stock on Yahoo Finance. View daily, weekly or monthly format back to when Netflix, Inc. stock was issued." property="og:description"/>,
     <meta content="Netflix, Inc. (NFLX) Stock Historical Prices &amp; Data - Yahoo Finance" property="og:title"/>,
     <meta content="Discover historical prices for NFLX stock on Yahoo Finance. View daily, weekly or monthly format back to when Netflix, Inc. stock was issued." property="twitter:description"/>,
     <meta content="Netflix, Inc. (NFLX) Stock Historical Prices &amp; Data - Yahoo Finance" property="twitter:title"/>,
     <meta content="328412701" property="al:ios:app_store_id"/>,
     <meta content="Yahoo Finance" property="al:ios:app_name"/>,
     <meta content="intent://quote/NFLX/#Intent;scheme=yfinance;action=android.intent.action.VIEW;package=com.yahoo.mobile.client.android.finance;S.browser_fallback_url=https%3A%2F%2Fplay.google.com%2Fstore%2Fapps%2Fdetails%3Fid%3Dcom.yahoo.mobile.client.android.finance;end" property="al:android:url"/>,
     <meta content="Yahoo Finance" property="al:android:app_name"/>,
     <meta content="com.yahoo.mobile.client.android.finance" property="al:android:package"/>,
     <meta content="app-id=328412701, app-clip-bundle-id=com.yahoo.finance.clip-qsp, affiliate-data=ct=us.fin.mbl.smart-banner&amp;pt=9029, app-argument=https://finance.yahoo.com/quote/NFLX" name="apple-itunes-app"/>,
     <meta content="guce.yahoo.com" name="oath:guce:consent-host"/>,
     <link href="https://s.yimg.com/uc/finance/dd-site/css/app.3aaaa2fc.css" rel="stylesheet"/>,
     <link href="https://s.yimg.com/uc/finance/dd-site/css/atomic-light.f1f1fb39.css" rel="stylesheet"/>,
     <link href="https://s.yimg.com/uc/finance/dd-site/css/yahooSansFinance.9be97232.css" rel="stylesheet"/>,
     <script src="https://s.yimg.com/aaq/yc/2.9.0/en.js"></script>,
     <script src="https://s.yimg.com/uc/finance/dd-site/js/jsErrorBeacon.js"></script>,
     <script src="https://s.yimg.com/ss/rapid3.js"></script>,
     <script src="https://s.yimg.com/uc/finance/srchjs/1.0.45/js/finSearch.modern.js"></script>,
     <script defer="" src="https://s.yimg.com/uc/finance/dd-site/js/vendor.13b96d36cf3c0a544a60.modern.js"></script>,
     <script defer="" src="https://s.yimg.com/uc/finance/dd-site/js/common.b03df9d456464634b46c.modern.js"></script>,
     <script src="https://consent.cmp.oath.com/cmpStub.min.js"></script>,
     <script async="" src="https://consent.cmp.oath.com/cmp.js"></script>,
     <script src="https://s.yimg.com/rq/darla/4-8-0/js/g-r-min.js"></script>,
     <script>(function(html){var c = html.className;c += " JsEnabled";c = c.replace("NoJs","");html.className = c;})(document.documentElement);</script>,
     <script>!function(){var e,t,n,i,r={passive:!0,capture:!0},a=new Date,o=function(){i=[],t=-1,e=null,f(addEventListener)},c=function(i,r){e||(e=r,t=i,n=new Date,f(removeEventListener),u())},u=function(){if(t>=0&&t<n-a){var r={entryType:"first-input",name:e.type,target:e.target,cancelable:e.cancelable,startTime:e.timeStamp,processingStart:e.timeStamp+t};i.forEach((function(e){e(r)})),i=[]}},s=function(e){if(e.cancelable){var t=(e.timeStamp>1e12?new Date:performance.now())-e.timeStamp;"pointerdown"==e.type?function(e,t){var n=function(){c(e,t),a()},i=function(){a()},a=function(){removeEventListener("pointerup",n,r),removeEventListener("pointercancel",i,r)};addEventListener("pointerup",n,r),addEventListener("pointercancel",i,r)}(t,e):c(t,e)}},f=function(e){["mousedown","keydown","touchstart","pointerdown"].forEach((function(t){return e(t,s,r)}))},p="hidden"===document.visibilityState?0:1/0;addEventListener("visibilitychange",(function e(t){"hidden"===document.visibilityState&&(p=t.timeStamp,removeEventListener("visibilitychange",e,!0))}),!0);o(),self.webVitals={firstInputPolyfill:function(e){i.push(e),u()},resetFirstInputPolyfill:o,get firstHiddenTime(){return p}}}();
     </script>,
     <script>!function(e,s,f,p){var a=[],t={_version:"3.11.4",_config:{classPrefix:"",enableClasses:!0,enableJSClass:!0,usePrefixes:!0},_q:[],on:function(e,t){var n=this;setTimeout(function(){t(n[e])},0)},addTest:function(e,t,n){a.push({name:e,fn:t,options:n})},addAsyncTest:function(e){a.push({name:null,fn:e})}},l=function(){};l.prototype=t,l=new l;var d=[];function v(e,t){return typeof e===t}var n="Moz O ms Webkit",c=t._config.usePrefixes?n.split(" "):[];t._cssomPrefixes=c;var y=f.documentElement,m="svg"===y.nodeName.toLowerCase();function h(){return"function"!=typeof f.createElement?f.createElement(arguments[0]):m?f.createElementNS.call(f,"http://www.w3.org/2000/svg",arguments[0]):f.createElement.apply(f,arguments)}var r={elem:h("modernizr")};l._q.push(function(){delete r.elem});var g={style:r.elem.style};function o(e,t,n,r){var o,i,s,a,l,d="modernizr",c=h("div"),u=((l=f.body)||((l=h(m?"svg":"body")).fake=!0),l);if(parseInt(n,10))for(;n--;)(s=h("div")).id=r?r[n]:d+(n+1),c.appendChild(s);return(o=h("style")).type="text/css",o.id="s"+d,(u.fake?u:c).appendChild(o),u.appendChild(c),o.styleSheet?o.styleSheet.cssText=e:o.appendChild(f.createTextNode(e)),c.id=d,u.fake&&(u.style.background="",u.style.overflow="hidden",a=y.style.overflow,y.style.overflow="hidden",y.appendChild(u)),i=t(c,e),u.fake?(u.parentNode.removeChild(u),y.style.overflow=a,y.offsetHeight):c.parentNode.removeChild(c),!!i}function i(e){return e.replace(/([A-Z])/g,function(e,t){return"-"+t.toLowerCase()}).replace(/^ms-/,"-ms-")}function S(e,t){var n=e.length;if("CSS"in s&&"supports"in s.CSS){for(;n--;)if(s.CSS.supports(i(e[n]),t))return!0;return!1}if("CSSSupportsRule"in s){for(var r=[];n--;)r.push("("+i(e[n])+":"+t+")");return o("@supports ("+(r=r.join(" or "))+") { #modernizr { position: absolute; } }",function(e){return"absolute"===function(e,t,n){var r;if("getComputedStyle"in s){r=getComputedStyle.call(s,e,t);var o=s.console;null!==r?n&&(r=r.getPropertyValue(n)):o&&o[o.error?"error":"log"].call(o,"getComputedStyle returning null, its possible modernizr test results are inaccurate")}else r=!t&&e.currentStyle&&e.currentStyle[n];return r}(e,null,"position")})}return p}function T(e){return e.replace(/([a-z])-([a-z])/g,function(e,t,n){return t+n.toUpperCase()}).replace(/^-/,"")}l._q.unshift(function(){delete g.style});var u=t._config.usePrefixes?n.toLowerCase().split(" "):[];function w(e,t){return function(){return e.apply(t,arguments)}}function C(e,t,n,r,o){var i=e.charAt(0).toUpperCase()+e.slice(1),s=(e+" "+c.join(i+" ")+i).split(" ");return v(t,"string")||v(t,"undefined")?function(e,t,n,r){if(r=!v(r,"undefined")&&r,!v(n,"undefined")){var o=S(e,n);if(!v(o,"undefined"))return o}for(var i,s,a,l,d,c=["modernizr","tspan","samp"];!g.style&&c.length;)i=!0,g.modElem=h(c.shift()),g.style=g.modElem.style;function u(){i&&(delete g.style,delete g.modElem)}for(a=e.length,s=0;s<a;s++)if(l=e[s],d=g.style[l],~(""+l).indexOf("-")&&(l=T(l)),g.style[l]!==p){if(r||v(n,"undefined"))return u(),"pfx"!==t||l;try{g.style[l]=n}catch(e){}if(g.style[l]!==d)return u(),"pfx"!==t||l}return u(),!1}(s,t,r,o):function(e,t,n){var r;for(var o in e)if(e[o]in t)return!1===n?e[o]:v(r=t[e[o]],"function")?w(r,n||t):r;return!1}(s=(e+" "+u.join(i+" ")+i).split(" "),t,n)}t._domPrefixes=u,t.testAllProps=C;var x=function(e){var t,n=P.length,r=s.CSSRule;if(void 0===r)return p;if(!e)return!1;if((t=(e=e.replace(/^@/,"")).replace(/-/g,"_").toUpperCase()+"_RULE")in r)return"@"+e;for(var o=0;o<n;o++){var i=P[o];if(i.toUpperCase()+"_"+t in r)return"@-"+i.toLowerCase()+"-"+e}return!1};t.atRule=x;t.prefixed=function(e,t,n){return 0===e.indexOf("@")?x(e):(-1!==e.indexOf("-")&&(e=T(e)),t?C(e,t,n):C(e,"pfx"))};l.addTest("canvas",function(){var e=h("canvas");return!(!e.getContext||!e.getContext("2d"))});var P=t._config.usePrefixes?" -webkit- -moz- -o- -ms- ".split(" "):["",""];function _(e,t,n){return C(e,p,p,t,n)}t._prefixes=P,l.addTest("csspositionsticky",function(){var e="position:",t=h("a").style;return t.cssText=e+P.join("sticky;"+e).slice(0,-e.length),-1!==t.position.indexOf("sticky")}),t.testAllProps=_;var b="CSS"in s&&"supports"in s.CSS,E="supportsCSS"in s;l.addTest("supports",b||E),l.addTest("csstransforms3d",function(){return!!_("perspective","1px",!0)}),l.addTest("csstransitions",_("transition","all",!0)),l.addTest("history",function(){var e=navigator.userAgent;return!!e&&((-1===e.indexOf("Android 2.")&&-1===e.indexOf("Android 4.0")||-1===e.indexOf("Mobile Safari")||-1!==e.indexOf("Chrome")||-1!==e.indexOf("Windows Phone")||"file:"===location.protocol)&&(s.history&&"pushState"in s.history))}),l.addTest("inlinesvg",function(){var e=h("div");return e.innerHTML="<svg/>","http://www.w3.org/2000/svg"===("undefined"!=typeof SVGRect&&e.firstChild&&e.firstChild.namespaceURI)}),l.addTest("localstorage",function(){var e="modernizr";try{return localStorage.setItem(e,e),localStorage.removeItem(e),!0}catch(e){return!1}}),l.addTest("sessionstorage",function(){var e="modernizr";try{return sessionStorage.setItem(e,e),sessionStorage.removeItem(e),!0}catch(e){return!1}}),l.addTest("svg",!!f.createElementNS&&!!f.createElementNS("http://www.w3.org/2000/svg","svg").createSVGRect),function(){var t=h("video");l.addTest("video",function(){var e=!1;try{(e=!!t.canPlayType)&&(e=new Boolean(e))}catch(e){}return e});try{t.canPlayType&&(l.addTest("video.ogg",t.canPlayType('video/ogg; codecs="theora"').replace(/^no$/,"")),l.addTest("video.h264",t.canPlayType('video/mp4; codecs="avc1.42E01E"').replace(/^no$/,"")),l.addTest("video.h265",t.canPlayType('video/mp4; codecs="hev1"').replace(/^no$/,"")),l.addTest("video.webm",t.canPlayType('video/webm; codecs="vp8, vorbis"').replace(/^no$/,"")),l.addTest("video.vp9",t.canPlayType('video/webm; codecs="vp9"').replace(/^no$/,"")),l.addTest("video.hls",t.canPlayType('application/x-mpegURL; codecs="avc1.42E01E"').replace(/^no$/,"")),l.addTest("video.av1",t.canPlayType('video/mp4; codecs="av01"').replace(/^no$/,"")))}catch(e){}}(),function(){var e,t,n,r,o,i;for(var s in a)if(a.hasOwnProperty(s)){if(e=[],(t=a[s]).name&&(e.push(t.name.toLowerCase()),t.options&&t.options.aliases&&t.options.aliases.length))for(n=0;n<t.options.aliases.length;n++)e.push(t.options.aliases[n].toLowerCase());for(r=v(t.fn,"function")?t.fn():t.fn,o=0;o<e.length;o++)1===(i=e[o].split(".")).length?l[i[0]]=r:(l[i[0]]&&(!l[i[0]]||l[i[0]]instanceof Boolean)||(l[i[0]]=new Boolean(l[i[0]])),l[i[0]][i[1]]=r),d.push((r?"":"no-")+i.join("-"))}}(),delete t.addTest,delete t.addAsyncTest;for(var z=0;z<l._q.length;z++)l._q[z]();e.Modernizr=l}(window,window,document);</script>,
     <script>!function(e){function t(t){t.matches?e.classList&&(e.classList.remove("themelight"),e.classList.add("themedark")):e.classList&&(e.classList.remove("themedark"),e.classList.add("themelight"))}if(window&&"function"==typeof window.matchMedia){var s=window.matchMedia("(prefers-color-scheme: dark)");s.addListener(t),t(s)}}(document.documentElement);</script>,
     <style>/*! normalize.css v4.0.0 | MIT License | github.com/necolas/normalize.css */html{font-family:sans-serif;-ms-text-size-adjust:100%;-webkit-text-size-adjust:100%}body{margin:0}article,aside,details,figcaption,figure,footer,header,main,menu,nav,section,summary{display:block}audio,canvas,progress,video{display:inline-block}audio:not([controls]){display:none;height:0}progress{vertical-align:baseline}[hidden],template{display:none}a{background-color:transparent}a:active,a:hover{outline-width:0}abbr[title]{border-bottom:none;text-decoration:underline;text-decoration:underline dotted}b,strong{font-weight:inherit}b,strong{font-weight:bolder}dfn{font-style:italic}h1{font-size:2em;margin:.67em 0}mark{background-color:#ff0;color:#000}small{font-size:80%}sub,sup{font-size:75%;line-height:0;position:relative;vertical-align:baseline}sub{bottom:-.25em}sup{top:-.5em}img{border-style:none}svg:not(:root){overflow:hidden}code,kbd,pre,samp{font-family:monospace,monospace;font-size:1em}figure{margin:1em 40px}hr{box-sizing:content-box;height:0;overflow:visible}button,input,select,textarea{font:inherit;margin:0}optgroup{font-weight:700}button,input,select{overflow:visible}button,select{text-transform:none}[type=reset],[type=submit],button,html [type=button]{-webkit-appearance:button}button::-moz-focus-inner,input::-moz-focus-inner{border:0;padding:0}button:-moz-focusring,input:-moz-focusring{outline:1px dotted ButtonText}fieldset{border:1px solid silver;margin:0 2px;padding:.35em .625em .75em}legend{box-sizing:border-box;color:inherit;display:table;max-width:100%;padding:0;white-space:normal}textarea{overflow:auto}[type=checkbox],[type=radio]{box-sizing:border-box;padding:0}[type=number]::-webkit-inner-spin-button,[type=number]::-webkit-outer-spin-button{height:auto}[type=search]{-webkit-appearance:textfield}[type=search]::-webkit-search-cancel-button,[type=search]::-webkit-search-decoration{-webkit-appearance:none}table{border-collapse:collapse;border-spacing:0}td,th{padding:0}[type=button],[type=reset],[type=submit],button{cursor:pointer}[disabled]{cursor:default}[dir]{text-align:start}[role=button]{box-sizing:border-box;cursor:pointer}:link{text-decoration:none;color:#324fe1}:visited{color:#324fe1}a:hover{text-decoration:underline}abbr[title]{border:0;cursor:help}b{font-weight:400}blockquote{margin:0;padding:0}body{background:#fff;color:#000;font:13px/1.3 'Helvetica Neue',Helvetica,Arial,sans-serif;height:100%;text-rendering:optimizeLegibility;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}button{box-sizing:border-box;font:16px 'Helvetica Neue',Helvetica,Arial,sans-serif;line-height:normal;background-color:transparent;border-color:transparent}dd,dl,p,table{margin:0}fieldset{border:0;margin:0;padding:0}h1,h2,h3,h4,h5,h6{font-size:16px;margin:0}html{height:100%}i{font-style:normal}img{vertical-align:bottom}input{background-color:#fff;border:1px solid #ccc;box-sizing:border-box;font:16px 'Helvetica Neue',Helvetica,Arial,sans-serif;display:inline-block;vertical-align:middle}input[disabled]{cursor:default}input[type=checkbox],input[type=radio]{cursor:pointer;vertical-align:middle}input[type=file],input[type=image]{cursor:pointer}input:focus{outline:0;border-color:rgba(82,168,236,.8);box-shadow:inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgba(82,168,236,.6)}input::placeholder{color:rgba(0,0,0,.4);opacity:1}ol,ul{margin:0;padding-left:0;list-style-type:none}optgroup{font:16px 'Helvetica Neue',Helvetica,Arial,sans-serif}select{background-color:#fff;border:1px solid #ccc;font:16px 'Helvetica Neue',Helvetica,Arial,sans-serif;display:inline-block;vertical-align:middle}select[multiple],select[size]{height:auto}textarea{background-color:#fff;border:1px solid #ccc;box-sizing:border-box;font:16px 'Helvetica Neue',Helvetica,Arial,sans-serif;resize:vertical}textarea:focus{outline:0;border-color:rgba(82,168,236,.8);box-shadow:inset 0 1px 1px rgba(0,0,0,.075),0 0 8px rgba(82,168,236,.6)}html.onDemandFocusSupport:not(.keyboardFriendly) #Aside:focus,html.onDemandFocusSupport:not(.keyboardFriendly) #Main:focus,html.onDemandFocusSupport:not(.keyboardFriendly) #Navigation:focus,html.onDemandFocusSupport:not(.keyboardFriendly) a:focus,html.onDemandFocusSupport:not(.keyboardFriendly) button:focus{outline:0}.SpaceBetween{text-align:justify;line-height:0}.SpaceBetween:after{content:'';display:inline-block;width:100%;vertical-align:middle}.SpaceBetween>*{display:inline-block;vertical-align:middle;line-height:1.3}@media screen\9{a:hover .StretchedBox{background-color:#fff;opacity:0}}[class*=LineClamp]{-webkit-backface-visibility:hidden}.Sticky-on .Sticky{position:fixed!important}.Scrolling #MouseoverMask{position:fixed;z-index:1000;cursor:default}#atomic .Fz\(s\){font-size:13px}#atomic .Fz\(m\){font-size:15px}#atomic .Fz\(l\){font-size:18px}#atomic .Fz\(xl\){font-size:20px}.uh-search .expanded .react-autocomplete-results{display:block}.uh-search .react-autocomplete-results{display:none;background-color:#fff;border:1px solid #b3b3b3;border-top:0;margin-right:58px}.uh-search .react-autocomplete-result-item{padding:4px 0;font-size:18px;color:#040404;padding-left:10px;padding-right:10px}.uh-search .react-autocomplete-result-item.focused,.uh-search .react-autocomplete-result-item:hover{background-color:#202020}.superheroContentTrans-enter{opacity:.01;transition:opacity .3s ease-in}.superheroContentTrans-enter.superheroContentTrans-enter-active{opacity:1}.superheroContentTrans-leave{position:absolute!important;display:block!important;top:0;opacity:1;transition:opacity .3s ease-in}.superheroContentTrans-leave.superheroContentTrans-leave-active{opacity:.01}.superheroHighlight{transition:opacity .3s ease-in;transition:background .3s ease-in}.hero-slideshow-right a,.lightbox-slideshow-right a{color:#0078ff}.sdaLite #viewer-LDRB,.sdaLite #viewer-LDRB2,.sdaLite #viewer-LREC,.sdaLite #viewer-LREC2,.sdaLite #viewer-LREC3,.sdaLite #viewer-LREC4,.sdaLite #viewer-MAST,.sdaLite #viewer-MON,.sdaLite #viewer-MON2,.sdaLite .caas-da,.sdaLite .viewer-sda-container{display:none}</style>,
     <style>.tdv2-applet-canvass .action-appear,.tdv2-applet-canvass .action-enter{opacity:.01}.tdv2-applet-canvass .action-leave{opacity:1}.tdv2-applet-canvass .action-appear.action-appear-active,.tdv2-applet-canvass .action-enter.action-enter-active{opacity:1;transition:opacity .5s ease-in}.tdv2-applet-canvass .action-leave.action-leave-active{opacity:.01;transition:opacity 1s ease-in}.tdv2-applet-canvass .arrow_box,.tdv2-applet-canvass .arrow_box_tags{position:relative;background:#fff;border:1px solid #e0e4e9}.tdv2-applet-canvass .arrow_box_tags:after,.tdv2-applet-canvass .arrow_box_tags:before{border:solid transparent;content:" ";height:0;width:0;position:absolute;pointer-events:none}.tdv2-applet-canvass .arrow_box:after,.tdv2-applet-canvass .arrow_box:before{bottom:100%;left:50%}.tdv2-applet-canvass .arrow_box_tags:after,.tdv2-applet-canvass .arrow_box_tags:before{left:47px;top:100%}.tdv2-applet-canvass .arrow_box:after,.tdv2-applet-canvass .arrow_box_tags:after{border-color:rgba(232,235,234,0);border-width:7px;margin-left:-7px}.tdv2-applet-canvass .arrow_box:after{border-bottom-color:#fff}.tdv2-applet-canvass .arrow_box_tags:after{border-top-color:#fff}.tdv2-applet-canvass .arrow_box:before,.tdv2-applet-canvass .arrow_box_tags:before{border-color:rgba(224,228,233,0);border-width:8px;margin-left:-8px}.tdv2-applet-canvass .arrow_box:before{border-bottom-color:#e0e4e9}.tdv2-applet-canvass .arrow_box_tags:before{border-top-color:#e0e4e9}.Ff\(YahooSans\){font-family:"Yahoo Sans"!important}.commentsExpandedHideAd #render-target-default.render-target-active #Aside .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-default.render-target-active #YDC-Col2 .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-default.render-target-active .modalRight .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-default.render-target-active [id^=defaultLDRB2-][id$=sizer],.commentsExpandedHideAd #render-target-default.render-target-active [id^=defaultLREC2-][id$=sizer],.commentsExpandedHideAd #render-target-default.render-target-active [id^=defaultLREC3-][id$=sizer],.commentsExpandedHideAd #render-target-default.render-target-active [id^=defaultLREC4-][id$=sizer],.commentsExpandedHideAd #render-target-default.render-target-active [id^=defaultMON2-][id$=sizer],.commentsExpandedHideAd #render-target-modal.render-target-active #Aside .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-modal.render-target-active #YDC-Col2 .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-modal.render-target-active .modalRight .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-modal.render-target-active [id^=modalLDRB2-][id$=sizer],.commentsExpandedHideAd #render-target-modal.render-target-active [id^=modalLREC2-][id$=sizer],.commentsExpandedHideAd #render-target-modal.render-target-active [id^=modalLREC3-][id$=sizer],.commentsExpandedHideAd #render-target-modal.render-target-active [id^=modalLREC4-][id$=sizer],.commentsExpandedHideAd #render-target-modal.render-target-active [id^=modalMON2-][id$=sizer],.commentsExpandedHideAd #render-target-modal.render-target-active [id^=tgt][id*=SIDE][id$=Stream] .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-viewer.render-target-active #Aside .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-viewer.render-target-active #YDC-Col2 .controller[data-tp-beacon],.commentsExpandedHideAd #render-target-viewer.render-target-active [id^=viewerLDRB2-][id$=sizer],.commentsExpandedHideAd #render-target-viewer.render-target-active [id^=viewerLREC2-][id$=sizer],.commentsExpandedHideAd #render-target-viewer.render-target-active [id^=viewerLREC3-][id$=sizer],.commentsExpandedHideAd #render-target-viewer.render-target-active [id^=viewerLREC4-][id$=sizer],.commentsExpandedHideAd #render-target-viewer.render-target-active [id^=viewerMON2-][id$=sizer]{display:none}button,textarea{font-family:inherit}@font-face{font-family:"Yahoo Sans";src:url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Regular.eot);src:url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Regular.eot?#iefix) format("embedded-opentype"),url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Regular.woff2) format("woff2"),url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Regular.woff) format("woff");font-weight:400;font-style:normal}@font-face{font-family:"Yahoo Sans";src:url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Semibold.eot);src:url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Semibold.eot?#iefix) format("embedded-opentype"),url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Semibold.woff2) format("woff2"),url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Semibold.woff) format("woff");font-weight:600;font-style:normal}@font-face{font-family:"Yahoo Sans";src:url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Bold.eot);src:url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Bold.eot?#iefix) format("embedded-opentype"),url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Bold.woff2) format("woff2"),url(https://s.yimg.com/cv/ae/sports/fonts/2017/Yahoo_Sans-Bold.woff) format("woff");font-weight:700;font-style:normal}</style>,
     <script>if("serviceWorker" in navigator){window.addEventListener("load",function(){navigator.serviceWorker.register("/_service-worker.js");});}</script>,
     <style>#atomic .render-target-modal #YDC-UH{display:none}#atomic #render-target-modal,#atomic #render-target-viewer{opacity:0}#atomic.modal-postopen #render-target-modal,#atomic.viewer-postopen #render-target-viewer{opacity:1}#atomic.modal-postopen #render-target-mrt,#atomic.modal-postopen .render-target-default,#atomic.viewer-postopen #render-target-mrt,#atomic.viewer-postopen .render-target-default{max-height:100%;overflow:hidden}#render-target-mrt{position:absolute;width:100%}#atomic.default-to-modal-fade .render-target-default,#atomic.default-to-viewer-fade .render-target-default,#atomic.modal-to-default-fade .render-target-modal,#atomic.mrt-to-modal-fade #render-target-mrt,#atomic.mrt-to-viewer-fade #render-target-mrt,#atomic.viewer-to-default-fade .render-target-viewer{position:absolute}#atomic.default-to-modal-fade .render-target-modal{-webkit-animation:fadein .15s ease-out forwards;animation:fadein .15s ease-out forwards}#atomic.modal-to-default-fade .render-target-modal{-webkit-animation:fadeout .15s ease-in forwards;animation:fadeout .15s ease-in forwards}#atomic.default-to-viewer-fade .render-target-viewer,#atomic.modal-to-viewer-fade .render-target-viewer{-webkit-animation:fadein .25s ease-out forwards;animation:fadein .25s ease-out forwards}#atomic.viewer-to-default-fade .render-target-viewer,#atomic.viewer-to-modal-fade .render-target-viewer{-webkit-animation:fadeout .25s ease-in forwards;animation:fadeout .25s ease-in forwards}@-webkit-keyframes fadein{0%{opacity:0}100%{opacity:1}}@-webkit-keyframes fadeout{0%{opacity:1}100%{opacity:0}}@keyframes fadein{0%{opacity:0}100%{opacity:1}}@keyframes fadeout{0%{opacity:1}100%{opacity:0}}</style>,
     <style>#atomic .video-lightbox .tdv2-applet-canvass .comment-icon,#atomic .video-lightbox .tdv2-applet-canvass .sort-filter-button>svg{fill:#fff!important;stroke:#fff!important}#atomic .video-lightbox .tdv2-applet-canvass .comments-title,#atomic .video-lightbox .tdv2-applet-canvass .message-content>div,#atomic .video-lightbox .tdv2-applet-canvass .see-more-wrapper>div,#atomic .video-lightbox .tdv2-applet-canvass .sort-filter-button>span,#atomic .video-lightbox .tdv2-applet-canvass .username{color:#fff!important}#atomic .video-lightbox .tdv2-applet-canvass a.comment-form{border:none!important}#atomic .video-lightbox .tdv2-applet-canvass .more-button>span{color:#787d82!important}#atomic .video-lightbox .tdv2-applet-canvass .canvass-gifs input{width:135px!important}#atomic .video-lightbox .vp-playlist-container.vp-playlist-mode-right.vp-playlist-theme-dark,#atomic .video-lightbox .yvp-playlist-container.yvp-playlist-mode-right.yvp-playlist-theme-dark{background:#0c0c0c}#atomic .video-lightbox .video-container .vp-content,#atomic .video-lightbox .video-container .yvp-content{background:0 0}#atomic .video-lightbox .video-container.playlist-dimmed .vp-playlist-container,#atomic .video-lightbox .video-container.playlist-dimmed .yvp-playlist-container{cursor:none}#atomic .video-lightbox .video-container.playlist-dimmed .vp-playlist-container .vp-playlist-item,#atomic .video-lightbox .video-container.playlist-dimmed .yvp-playlist-container .yvp-playlist-item{cursor:none;opacity:.2;transition:all .4s ease-in-out;transition-delay:.2s}#atomic .video-lightbox .video-container.playlist-undimmed .vp-playlist-container .vp-playlist-item,#atomic .video-lightbox .video-container.playlist-undimmed .yvp-playlist-container .yvp-playlist-item{opacity:1;transition:all .4s ease-in-out;transition-delay:.2s}#atomic .video-lightbox .video-container.playlist-hidden .vp-playlist-container,#atomic .video-lightbox .video-container.playlist-hidden .yvp-playlist-container{opacity:0;transition:all .4s ease-in-out}#atomic .video-lightbox .video-container .vp-content.vp-browser-desktop.vp-state-video.vp-hide-controls .vp-html5-video,#atomic .video-lightbox .video-container .yvp-content.yvp-browser-desktop.yvp-state-video.yvp-hide-controls .yvp-html5-video{cursor:none}</style>,
     <script>if(!window.finWebCore){window.finWebCore=function l(e){var t=e.isModern,s=void 0===t||t,i=e.isDev,o=void 0!==i&&i,r=e.lang,a=void 0===r?n:r,d=e.devAssets,l=e.prodAssets,c=e.strings,u={},f=a.substring(a.lastIndexOf("-")+1);return{lang:a,region:f,store:{},intl:f.toLowerCase(),strings:c,assets:o?d:l,addScriptTag:function(e,t,s){if(e){var i=document.createElement("script");for(var o in i.setAttribute("src",e),i.setAttribute("type","text/javascript"),"function"==typeof s&&(i.onload=function(){s(!0)},i.onerror=function(){s(!1)}),t)o&&Object.prototype.hasOwnProperty.call(t,o)&&void 0!==t[o]&&i.setAttribute(o,t[o]);document.body.appendChild(i)}},addAsset:function(e){var t,i=arguments.length>1&&void 0!==arguments[1]?arguments[1]:{},r=i.async,a=void 0===r||r,n=i.defer,c=i.useModule,f=void 0!==c&&c,p=i.callback;if(u[e])"function"==typeof p&&p(!0);else if((t=o?d:l)&&0!==t.length){u[e]=!0;var h=t[0]&&t[0][e]&&t[0][e].mjs;f?(this.addScriptTag(h,{async:a,defer:n,type:"module"},p),t.length>1&&!o&&(h=t[1]&&t[1][e]&&t[1][e].js,this.addScriptTag(h,{async:a,defer:n,nomodule:!0},p))):(t.length>1&&!s&&(h=t[1]&&t[1][e]&&t[1][e].js),this.addScriptTag(h,{async:a,defer:n},p))}},reset:function(){u={}}}}({isModern:true,isDev:false,lang:'en-US',devAssets:{},prodAssets:[{"_staticFinProtobuf":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/_staticFinProtobuf.545131e7093df52f5d2c.mjs"},"chart":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/chart.78df87548b8b5ec410f2.mjs"},"finIcon":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/finIcon.caeb77f68a8cd1254caa.mjs"},"finYodlee":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/finYodlee.f78d2ae49987e8266f2d.mjs"},"marketSummary":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/marketSummary.e60be2165c4dae187064.mjs"},"marketTime":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/marketTime.b374f210b209c90f624e.mjs"},"navigation":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/navigation.553912c8f5210a9f6cd6.mjs"},"portfolio":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/portfolio.7a4a72a88bcc1b33cb36.mjs"},"quoteSummary":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/quoteSummary.ebe73eab0a3e4a337874.mjs"},"sparkLine":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/sparkLine.bdc0be330ac83c917abf.mjs"},"streamer":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/streamer.4a117a133f8747c1b0e5.mjs"},"xrayStocks":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/xrayStocks.780114c48b9c40cfe064.mjs"},"":{"mjs":"https://s.yimg.com/uc/finance/webcore/js/streamer.worker.b8ddc29e6653826c3a90.worker.mjs"}},{"_staticFinProtobuf":{"js":"https://s.yimg.com/uc/finance/webcore/js/_staticFinProtobuf.3ddf9e84f90599f99e90.js"},"chart":{"js":"https://s.yimg.com/uc/finance/webcore/js/chart.d1d6abb03291978d3a13.js"},"finIcon":{"js":"https://s.yimg.com/uc/finance/webcore/js/finIcon.ce047fe701a463e273b2.js"},"finYodlee":{"js":"https://s.yimg.com/uc/finance/webcore/js/finYodlee.6fb3a53f773e9e99767b.js"},"marketSummary":{"js":"https://s.yimg.com/uc/finance/webcore/js/marketSummary.40b4eae4f3128606fef6.js"},"marketTime":{"js":"https://s.yimg.com/uc/finance/webcore/js/marketTime.6ab932bfbd8fa43e90c7.js"},"navigation":{"js":"https://s.yimg.com/uc/finance/webcore/js/navigation.e1f732de5c32b7db6117.js"},"portfolio":{"js":"https://s.yimg.com/uc/finance/webcore/js/portfolio.847aab83d6949f939982.js"},"quoteSummary":{"js":"https://s.yimg.com/uc/finance/webcore/js/quoteSummary.ba09e54afcfb983b8004.js"},"sparkLine":{"js":"https://s.yimg.com/uc/finance/webcore/js/sparkLine.a52a09edc3fa8dc16544.js"},"streamer":{"js":"https://s.yimg.com/uc/finance/webcore/js/streamer.0ac98993f716c007864f.js"},"xrayStocks":{"js":"https://s.yimg.com/uc/finance/webcore/js/xrayStocks.372c6a85ef1e81f560f0.js"},"":{"js":"https://s.yimg.com/uc/finance/webcore/js/streamer.worker.b7c872e4d5ae6291a631.worker.js"}}],strings:{"AUTHENTICATING":"Authenticating","CANCEL":"Cancel","CLOSE":"Close","COMPANY_NAME":"Company name","CONFIRM":"Confirm","EDIT_LIST":"Edit list","REFRESH":"Refresh","HIDE_HOLDINGS":"Hide holdings","SHOW_HOLDINGS":"Show holdings","DAY_GAIN":"Day gain","TOTAL_GAIN":"Total gain","AS_OF":"As of {time}","ADD_SYMBOLS":"Add Symbols","CUSTOM":"Custom","DELETE":"Delete","DELETE_WATCHLIST":"Delete Watchlist","DONE":"Done","GET_STARTED_ADD_SYMBOLS":"Get started by searching for companies to add to your new watchlist.","N_SYMBOLS":"{n} symbols","TOP_HOLDINGS":"Top holdings","SEARCH_SYMBOLS":"Search for companies & symbols","SHOW_MORE":"Show more","SHOW_LESS":"Show less","SORT_BY":"Sort by:","SYMBOL":"Symbol","GET_HELP_WITH_PREMIUM":"Get help with Premium","PORTFOLIO_ONBOARD_TITLE":"Let's build your first watchlist!","PORTFOLIO_ONBOARD_DESC":"Get started by using the search bar to find your favorite companies to add to your watchlist.","LINK_BROKER_VISIT":"Link your broker account by visiting","LEARN_MORE":"Learn more","MARKETS_OPEN":"Market open.","MARKET_TIME_NOTICE_CLOSED":"As of {date} {time}. {marketState}","MARKET_TIME_NOTICE_CLOSED_SHORT":"At close: {date} {time}","MY_WATCHLIST":"My Watchlist","PORTFOLIOS_TOTAL":"Portfolios Total","POST_MARKET_NOTICE":"After hours:","POST":"Post","PRE":"Pre","PRE_MARKET_NOTICE":"Pre-Market:","PRIVACY":"Verizon Media Privacy","PROVIDE_FEEDBACK":"Provide Feedback","PROGRESS_TRACK":"{current} of {total}","TAKE_TOUR":"Take the tour","TERMS":"Verizon Media Terms","TRY_AGAIN":"Try again","TRY_OTHER_BROWSER":"Unfortunately broker linking is not currently supported on Chrome. Please try again with another browser","VIEW_ON_YAHOO_FINANCE":"View on Yahoo Finance","UNLINKING":"Unlinking your account","YAHOO_FINANCE":"Yahoo Finance","YODLEE_ERROR":"Something went wrong on our end. Please try again.","YODLEE_BANK_ERROR":"Sorry, we couldnât connect your bank account","YODLEE_TIMEOUT":"Your session has timed out. Please sign in again"}}); function initStreamer(){ window.finWebCore.addAsset('streamer',{async:true}); } if(document.readyState === 'interactive' || document.readyState === 'complete'){ initStreamer(); }else{window.addEventListener('DOMContentLoaded', initStreamer);}}</script>,
     <script>(function () {
     if (!window.YAHOO || !window.YAHOO.i13n || !window.YAHOO.i13n.Rapid) { return; }
     var rapidConfig = {"async_all_clicks":true,"click_timeout":300,"client_only":1,"compr_type":"deflate","keys":{"ver":"ydotcom","navtype":"server","pt":"utility","pct":"qsp","pstcat":"equities","pg_name":"history","rvt":"NFLX","ticker":"NFLX","mrkt":"us","site":"finance","lang":"en-US","colo":"bf1","_yrid":"a8a62ldge68q3","_rid":"a8a62ldge68q3","abk":""},"pageview_on_init":true,"query_parameters":true,"test_id":"fd-tm-autosub,fdw-searchAdsQSP-V3-test2-copy-copy,finance-us-web-xray-upsell-2","tracked_mods_viewability":[],"track_right_click":true,"viewability":true,"dwell_on":true,"perf_navigationtime":2,"perf_resourcetime":1,"webworker_file":"/__rapidworker-1.2.js","spaceid":95993639};
     window.rapidInstance = new window.YAHOO.i13n.Rapid(rapidConfig);
     })();</script>]



Netflix, Inc. (NFLX) Stock Historical Prices &amp; Data - Yahoo Finance

Using beautiful soup extract the table with historical share prices and store it into a dataframe named `amazon_data`. The dataframe should have columns Date, Open, High, Low, Close, Adj Close, and Volume. Fill in each variable with the correct data from the list `col`.



```python
amazon_data = pd.DataFrame(columns=["Date", "Open", "High", "Low", "Close", "Volume"])

for row in soup.find("tbody").find_all("tr"):
    col = row.find_all("td")
    date = col[0].text#ADD_CODE
    Open = col[1].text#ADD_CODE
    high = col[2].text#ADD_CODE
    low = col[3].text#ADD_CODE
    close = col[4].text#ADD_CODE
    adj_close = col[5].text#ADD_CODE
    volume = col[6].text#ADD_CODE

    
    amazon_data = amazon_data.append({"Date":date, "Open":Open, "High":high, "Low":low, "Close":close, "Adj Close":adj_close, "Volume":volume}, ignore_index=True)
```

Print out the first five rows of the `amazon_data` dataframe you created.



```python
amazon_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Volume</th>
      <th>Adj Close</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jun 01, 2021</td>
      <td>504.01</td>
      <td>536.13</td>
      <td>482.14</td>
      <td>528.21</td>
      <td>78,560,600</td>
      <td>528.21</td>
    </tr>
    <tr>
      <th>1</th>
      <td>May 01, 2021</td>
      <td>512.65</td>
      <td>518.95</td>
      <td>478.54</td>
      <td>502.81</td>
      <td>66,927,600</td>
      <td>502.81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Apr 01, 2021</td>
      <td>529.93</td>
      <td>563.56</td>
      <td>499.00</td>
      <td>513.47</td>
      <td>111,573,300</td>
      <td>513.47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mar 01, 2021</td>
      <td>545.57</td>
      <td>556.99</td>
      <td>492.85</td>
      <td>521.66</td>
      <td>90,183,900</td>
      <td>521.66</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Feb 01, 2021</td>
      <td>536.79</td>
      <td>566.65</td>
      <td>518.28</td>
      <td>538.85</td>
      <td>61,902,300</td>
      <td>538.85</td>
    </tr>
  </tbody>
</table>
</div>



<b>Question 2</b> What is the name of the columns of the dataframe



```python
Date, Open, High, Low, Close, Volume, Adj Close
```

<b>Question 3</b> What is the `Open` of the last row of the amazon_data dataframe?



```python
536.79
```

<h2>About the Authors:</h2> 

<a href="https://www.linkedin.com/in/joseph-s-50398b136/?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkPY0220ENSkillsNetwork23455606-2021-01-01">Joseph Santarcangelo</a> has a PhD in Electrical Engineering, his research focused on using machine learning, signal processing, and computer vision to determine how videos impact human cognition. Joseph has been working for IBM since he completed his PhD.

Azim Hirjani


## Change Log

| Date (YYYY-MM-DD) | Version | Changed By | Change Description |
| ----------------- | ------- | ---------- | ------------------ |

```
| 2021-06-09       | 1.2     | Lakshmi Holla|Added URL in question 3 |
```

\| 2020-11-10        | 1.1     | Malika Singla | Deleted the Optional part |
\| 2020-08-27        | 1.0     | Malika Singla | Added lab to GitLab       |

<hr>

## <h3 align="center"> © IBM Corporation 2020. All rights reserved. <h3/>

<p>

