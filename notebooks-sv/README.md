# Introduktion av Notebooks och Jupyter

En kort introduktion av Jupyter notebooks. Vi kommer köra JupyterLabs i en Docker-container som jag gjort och som finns på [Docker Hub](https://hub.docker.com/repository/docker/reuteras/container-notebook). Byggs automatiskt för Intel och Mac från [GitHub](https://github.com/reuteras/container-notebook). 

För att förenkla under presentationen så gör vi ett Docker network för att enkelt kunna dela nät med Elasticsearch senare.

	docker network create presentation

Starta en Docker container med Jupyter Notebook och en del verktyg installerade för att kunna se presentationen. På Windows fungerar följande (anpassa sökvägen och det gäller för Linux och Mac också):

	docker run --name notebook --network presentation --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v C:\presentations\notebooks-sv\home:/home/jovyan reuteras/container-notebook 

Instruktionerna fortsätter nu i notebooken med namnet _1_introduktion.ipynb_.

## Länkar

Nedan finns länkarna som förekommer i presentationen.

### Jupyter notebooks

- [Jupyter](https://jupyter.org/)
- [Awesome Jupyter](https://github.com/markusschanta/awesome-jupyter)
- [Jupyter Notebook: An Introduction](https://realpython.com/jupyter-notebook-introduction/)
- [How to Display Rich Media Contents (Image, Audio, Video, etc) in Jupyter Notebook?](https://coderzcolumn.com/tutorials/python/how-to-display-contents-of-different-types-in-jupyter-notebook-lab)
- [nbconvert: Convert Notebooks to other formats](https://nbconvert.readthedocs.io/en/latest/index.html)
- [The Jupyter+git problem is now solved](https://www.fast.ai/posts/2022-08-25-jupyter-git.html)


### MSTICPy

- [MSTICPy](https://github.com/microsoft/msticpy)
- MSTICPy [dokumentation](https://msticpy.readthedocs.io/en/latest/index.html)
- [Using Python to unearth a goldmine of threat intelligence from leaked chat logs](https://www.microsoft.com/en-us/security/blog/2022/06/01/using-python-to-unearth-a-goldmine-of-threat-intelligence-from-leaked-chat-logs/) och den [notebook](https://github.com/microsoft/msticpy/blob/main/docs/notebooks/ContiLeaksAnalysis.ipynb) som användes.

### Python och Elasticsearch

- [Python Elasticsearch Client](https://elasticsearch-py.readthedocs.io/en/latest/) documentation
- [Elasticsearch DSL](https://elasticsearch-dsl.readthedocs.io/en/latest/index.html) documentation

### Pandas

- [Pandas](https://pandas.pydata.org/)
- [The Pandas DataFrame: Make Working With Data Delightful](https://realpython.com/pandas-dataframe/)

### Övrigt

- [memOptix](https://github.com/blueteam0ps/memOptix)
- [Learning Packet Analysis with Data Science](https://medium.com/hackervalleystudio/learning-packet-analysis-with-data-science-5356a3340d4e)
- [Explorations of PCAP files from contagio malware dump](https://anaconda.org/anaconda-enterprise/malware-traffic-analysis/notebook)
- [Jupyter Notebook applied to cybersecurity and threat intelligence](https://jupyter.securitybreak.io/) - innehåller bland annat följande notebooks, _ELK Threat Hunting_, _VT Domain Hunting using MSTICpy_, _10 Python Libs for Malware Analysis and Reverse Engineering_ och _IoCExtractor using MSTICpy_.
- [11 Jupyter Notebook Techniques You Should Know](https://ai.plainenglish.io/11-jupyter-notebook-techniques-you-should-know-2ebeafefa303)

Koden för att hämta data från Flashback finns här:

- [https://github.com/christopherkullenberg/flashbackscraper](https://github.com/christopherkullenberg/flashbackscraper) 
