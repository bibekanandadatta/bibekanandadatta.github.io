---
layout: post
title: Licensing and citing contents from online repositories
date: 2024-05-02 09:00:00-0400
description: Some common resources to understand licensing and citing of online contents or repositories
tags: licensing open-source GitHub
categories: resources
giscus_comments: false
related_posts: true
# toc:
#   sidebar: left
---

GitHub is phenomenal when it comes about hosting code repositories with `Git` version control system specially for open-source contents. I have been benefited from it like most others and want to promote the same culture as well. However, recently, I ran into a slightly unconventional issue regarding that matter. I published two different source codes with documentations as educational resources which I wanted to be available for free for learning purposes without violating academic integrity.


## Licensing

> ##### WARNING
>
> This is not a legal advice; rather it is just my experience of finding out resources related to licensing.
{: .block-warning }


GitHub created the website <https://choosealicense.com> to help its users to educate about different licenses. Some other common open source licenses are GNU GPL, Apache License, BSD License, Creative Commons, etc. While this website is a good starting point, it does not have enough details. GNU has a dedicated webpage listing different licenses with their opnions (<https://www.gnu.org/licenses/license-list.html>). Two other somewhat similar resources are by Free Software Foundation (<https://www.fsf.org/licensing/>) and Open Source Initiative (<https://opensource.org/licenses>). If you are not very well-versed with jargons and lingos used in these license, then <https://www.tldrlegal.com> got you covered with quick overviews of common licenses.

License is considered as a contract between the copyright holder and the public which both parties need to follow. Some of the licenses are very permissible and you can change and relicense your work later, but some other licenses are not that permissible or irevocable even by the copyright holder. Some licenses are compatible with each other and some are not. Considering that, you should read the licensing deed/ agreement thoroughly and consult with appropriate personels before applying any license to your work. At the same time you should be careful of using other works as well. You should read and understand what freedom the copyright holder provides you as a user. If you have been through the websites mentioned above you should be familiar with some legal terms and your responsibilities as user by now. If you still not convinced what can happen, I urge you to read [this blogpost](https://www.nafems.org/blog/posts/analysis-origins-msc-and-nastran/). I am sure there are plenty other stories like this.

Going back to my repositories, after spending a few days reading through different licesning agreements, I decided to relicense my work under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). This license is common for educational resources such as MIT uses it for their Open Courseware contents. But please note, creative Commons discourages using this license for software product and technically it can not be called "open-source" when non-commerical clause is attached to it.



## DOI and citation

The second step of this process was to make the repository citable if someone develops further work based on that. Other than traditional journals, there are a few website that allow depositing data, code, documentation type works and receive a DOI against that. [This blogpost](https://evodify.com/free-research-repository/) will provide you a quick overview of services like [DRYAD](https://datadryad.org/stash), [Zenodo](https://zenodo.org), [FigShare](https://zenodo.org), [Mendeley](https://data.mendeley.com), [Open Science Framework](https://osf.io), etc. This post is about 5 years old by now, so you should carefully look into the pros and cons of different services.

Anyway, since GitHub allows direct integration with Zenodo with versioning system, I opted-in for that. [This GitHub documentation](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content) will walk you throught that process. Briefly, you will need connect your GitHub repository to your Zenodo account and then you will have to create a release from your repository which will trigger Zenodo to archieve a `.zip` version of your repository and give you DOI for that release.



> ##### CAUTION
>
> In this approach, once you create a release on GitHub, you will get a new DOI every single time from Zenodo. For each DOI, you can change the metadata (description, date, webpage, etc.), but you can not change the files once the DOI is generated. One other pitfall is that you do not get the DOI ahead of time to include it in the README file before creating the release.
{: .block-danger }


For these reasons, I eventually decided I will turn off GiHub-Zenodo integration and manually create a DOI from Zenodo. My workflow is, I start a **New Upload** process and reserve a DOI immediately. Once the DOI is available, I include that DOI in the README file and other documentation of my project. Followed by that, I fill out other meta data (Title, Resource type, Creators, License, Webpage, Resource type, etc.) and upload the zip files and check the project from the **Preview**. You can edit and delete the record in draft mode. Once everything looks good to me, I publish the record. While the files can not be changed, metadata associated with the project can be updated later while keeping the DOI. Once a record is published, it is somewhat difficult to remove it. Zenodo gives a month long grace period for users to request record deletion but some services do not provide that. So publish the record carefully.


Like GitHub release feature, Zenodo allows versioning system for the work and will create a new DOI everytime a new version of the files are uploaded. But Zenodo provides **all version DOI** which will always redirect to the latest version of the record. If you release more than one version of the records, you can include that DOI in the second version. Follow [semantic versioning](https://semver.org) for your record or release. In this approach, X.Y.Z numeric formats are used to represent a version whcih you may have seen before for different software or packages. Briefly,

- X is for major release when incompatibility between the previous and new release happens
- Y is for minor release when new functionalities are added to the previous release in a backward compatible manner
- Z is for compatible bug fixes

This was my quick attempt to go over some really complicated issues which are not well-understood and discussed among students within academic settings. In my opinion, these issues are rabbithole once you get into it. Be sure to discuss with appropriate personel for these.


> ##### TIP
>
> If you are a part of an institution, you should definitely discuss with your manager or advisor and technology transfer office before you apply a license to your work or relicense your work. 
> You should also check with your institution about DOI services for your work (archieving data, code, or documentation).
{: .block-tip }