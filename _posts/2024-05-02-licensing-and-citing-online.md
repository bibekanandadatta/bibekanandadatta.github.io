---
layout: post
title: Licensing and citing academic software and tutorials
date: 2024-05-02 09:00:00-0400
description: Some common resources to understand licensing and citing online contents
tags: licensing open-source DOI Zenodo GitHub
categories: resources
giscus_comments: false
related_posts: true
# toc:
#   sidebar: left
---

GitHub is amazing when it comes to hosting code repositories with the `Git` version control system. As of early 2024, almost everyone, ranging from commercial entities to academic researchers, is sharing their works on GitHub. This has benefited me greatly in terms of learning. However, recently I ran into a rather unconventional issue that I did not know much about previously. I published two different source codes with documentation as educational resources which I wanted to be available for free for learning purposes without violating academic integrity. The viable way to ensure this is to share the contents in a public repository with appropriate licensing and make it citable by others.


## Licensing

> ##### WARNING
>
> This is not legal advice; rather it is just my experience of finding out and sharing resources related to licensing academic content.
{: .block-warning }

License is considered as a contract between the copyright holder and the public which both parties need to follow. Some licenses are very permissible and compatible with other licenses and some others are more restrictive and incompatible with many common licenses. You should read the licensing deed/ agreement thoroughly and consult with the appropriate personnel before applying any license to your work. At the same time, when you are using someone else's code or program, you should read and understand what freedom the copyright holder provides you as a user. [This blog post on the origin of MSC.Nastran software](https://www.nafems.org/blog/posts/analysis-origins-msc-and-nastran/) has a section on their legal troubles with NASA. This should motivate you to look more closely into the licensing.



### Some resources on getting started with licensing

GitHub created a simple website <https://choosealicense.com> to help its (new) users educate about different licenses. This website is a good starting point if you have a small project, but it does not have enough details for many common licenses that are used frequently by many others. GNU also has a dedicated webpage listing different licenses with their opinions (<https://www.gnu.org/licenses/license-list.html>). Two other somewhat similar web pages are by Free Software Foundation (<https://www.fsf.org/licensing/>) and Open Source Initiative (<https://opensource.org/licenses>). If you are not very well-versed with the jargon and lingos used in these licenses, then <https://www.tldrlegal.com> got you covered with quick overviews of common licenses.



### Shared experiences by other academics

If you are a new developer who is primarily developing scientific software for research purposes, you may be unaware of such complexities and confused after reading the first paragraph. I would like to ensure that you are not alone. Others have been in our shoes before; here are a few blog posts written by some professors in the past decade on this issue. These blogs are well-written with more concrete examples and first-hand experiences.

- Jake VanderPlas's blog: <https://www.astrobetter.com/blog/2014/03/10/the-whys-and-hows-of-licensing-scientific-code/>.
- Prof. C. Titus Brown's blog: <http://ivory.idyll.org/blog/2015-on-licensing-in-bioinformatics.html>.
- Prof. Lior S. Patcher's blog: <https://liorpachter.wordpress.com/2017/08/03/i-was-wrong-part-2/>.


After reading those blogs you probably realized that there is no general consensus among academics and software developers on licensing; everyone has different philosophies and preferences. Some prefer MIT or BSD-like permissive licenses which will allow the users to do whatever they want, and some prefer GNU GPL-like copyleft licenses which implicitly pose some restriction on how the source code and software needs to be distributed.


### My viewpoint

I think, choosing a license primarily depends on your philosophy of how would you want to make your project available and adopted by others. It is best to sit down and think about the broader objectives of the project that you want to share. Some of the questions you should ask yourself are:

- What are the broad objectives of your work?
- Do you have any restrictions on how the code is to be shared by the funding agency?
- Do you want other people to use your work without any modification? Or do you want other people to adopt and extend your work for their applications?
- Do you care about being properly credited for the work if someone adopts the work and uses it in their project?
- Are you okay with someone else developing proprietary (or commercial) products based on your work while you may or may not be paid for it?

Based on the answers to these questions, you can choose a permissive license, a copyleft license, a dual-license, or something else.

I asked these questions to myself and my answers were I consider the work I mentioned in the beginning to be an educational resource that includes a source code and some test cases demonstrating an application specific to a framework. I would like others to learn from this and build on top of this. However, I do not want any student to copy-paste the work and pass it on as their own for academic/ research work and someone to distribute it for any monetary gain. Based on those, I decided to relicense my work under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) which is somewhat commonly used for educational resources (e.g., MIT Open Courseware). However, I should mention that the Creative Commons licenses are not suitable for software. But my repository was less of software and more of a tutorial that has source code to demonstrate the tutorial. While it was possible to license the software and the code under different agreements, it seemed a bit more complicated for me at that time, but I may chose to do it in future.

>
> A fun fact is when a *non-commercial* clause is added to any license, it can no longer be considered as open-source by definition. You can use other terms such as *source-available* for such content.


### TL;DR

- Do not use any work that is unlicensed even if it is publicly shared online.
- Properly credit the original creator if their license requires you to do so.
- Attach a license that you think suits the goal of the project and your philosophies.
- Talk to appropriate personnel regarding this before making any decision.


## Citation using digital object identifier (DOI)

The second step of this process was to make the GitHub repository citable when they use it in an academic context in the future. Yes, it is possible to cite a GitHub repository directly, but hyperlinks are never permanent. At some point, it will probably break or change. This is where the concept of digital object identifier (DOI) comes in. DOI points to an archived object on the internet permanently regardless of the hyperlink and is commonly used by academic journals and preprints among others. Other than the traditional academic publications, there are a few websites that allow general users to deposit their data, code, and documentation-type works and receive a DOI against that. [This blog post](https://evodify.com/free-research-repository/) will provide you a quick overview of services like [DRYAD](https://datadryad.org/stash), [Zenodo](https://zenodo.org), [FigShare](https://zenodo.org), [Mendeley](https://data.mendeley.com), [Open Science Framework](https://osf.io), etc. This blog post is about 5 years old by now, so you should carefully look into the currently available features provided by different services. Even a lot of universities provide data archival services to their students, staff, and faculties.


### Using Zenodo


Since GitHub allows direct integration with Zenodo with the versioning system, I opted in for that. Zenodo is maintained by CERN and claims to be in existence as long as CERN exists. [This GitHub documentation](https://docs.github.com/en/repositories/archiving-a-github-repository/referencing-and-citing-content) will walk you through that process. Briefly, you will need to connect your GitHub repository to your Zenodo account and then you will have to create a release from your repository which will trigger Zenodo to archive a `.zip` version of your repository and give you DOI for that release.


> ##### CAUTION
>
> In this approach, once you create a release on GitHub, you will get a new DOI every single time from Zenodo. For each DOI, you can change the metadata (description, date, webpage, etc.), but you can not change the files once the DOI is generated. One other pitfall is that you do not get the DOI ahead of time to include it in the README file before creating the release.
{: .block-danger }

For these reasons, I eventually decided I would turn off GiHub-Zenodo integration and manually create a DOI from Zenodo. My workflow on Zenodo is that I start a **New Upload** process and reserve a DOI immediately. Once the DOI is available, I include that DOI in the README file and other documentation of my project. Following that, I fill out other metadata (Title, Resource type, Creators, License, Web page, Resource type, etc.), upload the zip files, and check the project from the **Preview**. You can edit and delete the record in draft mode. Once everything looks good to me, I publish the record. While the files can not be changed, metadata associated with the project can be updated later while keeping the DOI. Once a record is published, it is somewhat difficult to remove it. Zenodo gives a month-long grace period for users to request record deletion but some services do not provide that. So publish the record carefully.


Similar to the GitHub release feature, Zenodo allows a versioning system for the work and will create a new DOI every time a new version of the files is uploaded. But Zenodo also provides **all version DOI** which will always redirect to the latest version of the record. If you release more than one version of the records, you can include the **all version DOI** in the README file or documentation of the second version and later. When you upload a new version on Zonedo, try following [semantic versioning](https://semver.org) for your record or release. In this approach, X.Y.Z numeric formats are used to represent a version that you may have seen before for different software or packages. Briefly,

- X is for major releases when incompatibility between the previous and new releases happens
- Y is for minor release when new functionalities are added to the previous release in a backward compatible manner
- Z is for compatible bug fixes

This was my quick attempt to go over some really complicated issues that are not well-understood and discussed among students within academic settings especially those who develop programs or software or documentation. In my opinion, these issues are a deep rabbit hole once you get into it. So, be sure to discuss with appropriate personnel to avoid any sort of aftermath.


> ##### TIP
>
> If you are a part of an institution, you should definitely discuss this with your manager or advisor and technology transfer office before you apply any license to your work or relicense it later. 
> You should also check with your institution about archival services and obtaining DOI.
{: .block-tip }