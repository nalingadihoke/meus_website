---
title: NSF Big-Data Theoretic
summary: Extracting and cleaning Event Reports (OCR text data) from Nuclear Power Plants in the U.S.
tags: [Data Cleaning, Data Extraction]
categories: [web scraping, data cleaning, research, nuclear]
date: "2020-01-31"
share: false


image:
  caption: 
  focal_point: Smart

links:
#- icon: twitter
#  icon_pack: fab
#  name: Follow
#  url: https://twitter.com/georgecushen
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

---

As part of the [Soteria lab](https://soteria.npre.illinois.edu/) team I worked on the web scraping of the Nuclear Regulatory Commission’s (NRC) website for event reports from nuclear power plants and was involved in the subsequent cleaning and management of this text data. Part of the NSF-funded “Big-Data Theoretic” project, I worked under the guidance of Justin Pence and my work was acknowledged in two journal articles and his Ph.D. dissertation. You can find the articles [here](https://doi.org/10.1016/j.ssci.2019.104574) and [here](https://doi.org/10.1016/j.ress.2018.12.020).  

Risk Assessment tends to be very important when it comes to nuclear power plants. In industries like nuclear and aerospace, catastrophic failure tends to be a low-frequency high-consequence event. As opposed to deterministic risk assessment, Probabilistic Risk Assessment ([PRA](https://en.wikipedia.org/wiki/Probabilistic_risk_assessment)) is a comprehensive methodology to model common cause failures, human errors and assumes even rare events can occur.  The end goal for analyzing the text data was to try and find organizational factors that contribute to undesirable events which would then be quantified and used as a base input into PRA. The text mining after our initial extraction and pre-processing was carried out by [collaborators](https://ischool.illinois.edu/people/catherine-blake) in the iSchool, UIUC.

## Extraction

As any good web-scraper knows, webpage variability is the bane of many a well written script. And government websites exemplify this the best. However, as much as I would like to show the website we had to deal with, the NRC has since made amends and the new website is much [better](https://www.nrc.gov/site-help/ler-message.html). For my work, the presence of hidden dropdown menu’s and other such webpage elements meant [Selenium Webdriver](https://selenium-python.readthedocs.io/) had to be used. On the data managent side, I’d recommend checking out [shutil](https://docs.python.org/3/library/shutil.html) for incorporating file operations into any script; it was very useful in dealing with over 15k event reports spanning 3 decades.

## Cleaning

Event Reports are generated for every small noticeable event that happens in the system in question. From pipe breakage to an alarm system malfunctioning, 100s of such reports are generated daily in power plants across the US.

{{< figure src="event_report.png" title="Example of an event report" lightbox="true" >}}

These event reports were available as text files which were the result of OCR. Now, OCR on the above shown example does not yield a pretty result. The files had all kinds of extraneous text mixed in with the actual meat of the report – sections such as title, description, conclusion etc. There were some common denominators such as table properties and these were eliminated using regular expressions. The remainder of the illegible text was dealt with using [nltk](https://www.nltk.org/index.html) and NLP methods.

Further Reading:
- Justin's thoughts on risk informed internet regulation - his [medium article](https://medium.com/swlh/internetriskregulation-a01000401d94) and [website](https://www.orgz.info/).
- Big-Data Theoretic project [page](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1535167&HistoricalAwards=false).
- [Dr Zahrah Mohaghegh](https://npre.illinois.edu/people/profile/zahra13) is the Soteria Lab PI and her focus is PRA in nuclear systems.


