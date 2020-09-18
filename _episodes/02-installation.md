---
title: "Installation and Execution"
teaching: 0
exercises: 15
questions:
- "How do I install CMSSW?"
- "How do I execute CMSSW?"
- "How do I compile CMSSW software?"
objectives:
- "Review the steps necessary to setup a CMSSW area"
- "Learn how to compile CMSSW and find out about its executable"

keypoints:
- "A CMSSW area is not really installed but set up."
- "`cmsRun` is the CMSSW executable.  There are also utilitarian scripts."
- "You can compile CMSSW with `scram b`"
---

## Setting up your CMSSW area

If you completed the lessons on [virtual machines](https://cms-opendata-workshop.github.io/workshop-lesson-virtualmachine/) or [Docker](https://cms-opendata-workshop.github.io/workshop-lesson-docker/) you already have a working CMSSW area.  You can start it over by either turning on your virtual machine and going to the right shell according to the [validation instructions](run-a-simple-demo-for-testing-and-validating) or, if using Docker, by starting the container:

~~~
docker start -i <theNameOfyourContainer>
~~~
{: .language-bash}

Make sure you change directories to the `CMSSW_5_3_32/src` area; for instance, in Docker:

~~~
cd /home/cmsusr/CMSSW_5_3_32/src
~~~
{: .language-bash}

Note that we are not really "installing" CMSSW but setting up an environment for it.  CMSSW was already installed. This is why **every time** you open a new shell you will have to issue the `cmsenv` command, which is just a script that runs to set some environmental variables for your working area:

~~~
cmsenv
~~~
{: .language-bash}

Now you can check, for instance, where your `CMSSW_RELEASE_BASE` variable points to:

~~~
echo $CMSSW_RELEASE_BASE
~~~
{: .language-bash}

The variable may point to a local CMSSW install if you are using a Docker container:

~~~
/opt/cms/slc6_amd64_gcc472/cms/cmssw/CMSSW_5_3_32
~~~
{: .output}

or to a place in the shared cvmfs area if working in a virtual machine:
~~~
/cvmfs/cms.cern.ch/slc6_amd64_gcc472/cms/cmssw/CMSSW_5_3_32
~~~
{: .output}


## **cmsRun**, the CMSSW executable

All the packages that comprise the CMSSW release in use have been already compiled and linked to one single **executable**, which is called `cmsRun`.  So, unless you want to create your own plugin (addition) for the software, you won't not even have to re-compile.  You can actually try to execute this command by itself, but it will give you a configuration error:

~~~
cmsRun
~~~
{: .language-bash}

~~~
cmsRun: No configuration file given.
For usage and an options list, please do 'cmsRun --help'.
~~~
{: .error}

So, inevitably, the cmsRun executable needs a configuration file.  This configuration file must be written in Python.  Do not worry, we will learn all about configuration in a next episode of this lesson.

> ## Run with a configuration
>
> Now try to run, but this time with a configuration file.
>
> > ## Solution
> >
> > We could simply repeat what we already did while setting up our VM or container: run with the `Demo/DemoAnalyzer/demoanalyzer_cfg.py` python configuration file.  However,
> > this time we could store the output in a `dummy.log` file and run it in the background.  Notice the bash redirector `>`, the redirection of `stderr` to `stdout` (`2>&1`), and the trailing run-in-the-background control operator `&`.  
> >
> > ~~~
> > cmsRun Demo/DemoAnalyzer/demoanalyzer_cfg.py > dummy.log 2>&1 &
> > ~~~
> > {: .language-bash}
> >
> > You can check the development of your job with
> >
> > ~~~
> > tail -f dummy.log
> > ~~~
> >
> > When finished, if you dump the content of `dummy.log`
> >
> > ~~~
> > cat dummy.log
> > ~~~
> >
> > you'll get
> >
> > ~~~
> > 18-Sep-2020 06:11:41 CEST  Initiating request to open file root://eospublic.cern.ch//eos/opendata/cms/Run2011A/ElectronHad/AOD/12Oct2013-v1/20001/001F9231-F141-E311-8F76-003048F00942.root
> > 18-Sep-2020 06:11:44 CEST  Successfully opened file root://eospublic.cern.ch//eos/opendata/cms/Run2011A/ElectronHad/AOD/12Oct2013-v1/20001/001F9231-F141-E311-8F76-003048F00942.root
> > Begin processing the 1st record. Run 166782, Event 340184599, LumiSection 309 at 18-Sep-2020 06:11:53.936 CEST
> > Begin processing the 2nd record. Run 166782, Event 340185007, LumiSection 309 at 18-Sep-2020 06:11:53.937 CEST
> > Begin processing the 3rd record. Run 166782, Event 340187903, LumiSection 309 at 18-Sep-2020 06:11:53.937 CEST
> > Begin processing the 4th record. Run 166782, Event 340227487, LumiSection 309 at 18-Sep-2020 06:11:53.937 CEST
> > Begin processing the 5th record. Run 166782, Event 340210607, LumiSection 309 at 18-Sep-2020 06:11:53.937 CEST
> > Begin processing the 6th record. Run 166782, Event 340256207, LumiSection 309 at 18-Sep-2020 06:11:53.938 CEST
> > Begin processing the 7th record. Run 166782, Event 340165759, LumiSection 309 at 18-Sep-2020 06:11:53.938 CEST
> > Begin processing the 8th record. Run 166782, Event 340396487, LumiSection 309 at 18-Sep-2020 06:11:53.938 CEST
> > Begin processing the 9th record. Run 166782, Event 340390767, LumiSection 309 at 18-Sep-2020 06:11:53.939 CEST
> > Begin processing the 10th record. Run 166782, Event 340435263, LumiSection 309 at 18-Sep-2020 06:11:53.939 CEST
> > 18-Sep-2020 06:11:54 CEST  Closed file root://eospublic.cern.ch//eos/opendata/cms/Run2011A/ElectronHad/AOD/12Oct2013-v1/20001/001F9231-F141-E311-8F76-003048F00942.root
> >
> > =============================================
> >
> > MessageLogger Summary
> >
> >  type     category        sev    module        subroutine        count    total
> >  ---- -------------------- -- ---------------- ----------------  -----    -----
> >     1 fileAction           -s file_close                             1        1
> >     2 fileAction           -s file_open                              2        2
> >
> >  type    category    Examples: run/evt        run/evt          run/evt
> >  ---- -------------------- ---------------- ---------------- ----------------
> >     1 fileAction           PostEndRun                        
> >     2 fileAction           pre-events       pre-events       
> >
> > Severity    # Occurrences   Total Occurrences
> > --------    -------------   -----------------
> > System                  3                   3
> >
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

## Compilation

You could also try to compile (**b**uild) the code.  We use `scram`, the release management tool used for CMSSW:

~~~
scram b
~~~
{: .language-bash}

~~~
Reading cached build data
>> Local Products Rules ..... started
>> Local Products Rules ..... done
>> Building CMSSW version CMSSW_5_3_32 ----
>> Subsystem FT_53_LV5_AN1 built
>> Entering Package Demo/DemoAnalyzer
>> Creating project symlinks
  src/Demo/DemoAnalyzer/python -> python/Demo/DemoAnalyzer
>> Leaving Package Demo/DemoAnalyzer
>> Package Demo/DemoAnalyzer built
>> Subsystem Demo built
>> Local Products Rules ..... started
>> Local Products Rules ..... done
gmake[1]: Entering directory `/home/cmsusr/test/CMSSW_5_3_32'
>> Creating project symlinks
  src/Demo/DemoAnalyzer/python -> python/Demo/DemoAnalyzer
>> Done python_symlink
>> Compiling python modules cfipython/slc6_amd64_gcc472
>> Compiling python modules python
>> Compiling python modules src/Demo/DemoAnalyzer/python
>> All python modules compiled
>> Pluging of all type refreshed.
>> Done generating edm plugin poisoned information
gmake[1]: Leaving directory `/home/cmsusr/test/CMSSW_5_3_32'
~~~
{: .output}

Note that scram only goes into the `Demo/DemoAnalyzer` package that we created locally to validate our setup in a previous lesson.  All the rest of the packages in the release were already compiled.  Since there is nothing new to compile, it finishes very quickly.  In a later episode we will modify this `DemoAnalyzer` and will need to compile again.

## Additional goodies

Your CMSSW environment comes with other executable scripts/tools that can be very useful.  An example of those is the `mkedanlzr` script that we use already to create the `DemoAnalyzer` package.  This script creates skeletons for EDAnalyzers that can later be modified or expanded.  Notice that this package, DemoAnalyzer, has a similar structure as any of the CMSSW packages we mentioned [before](../01-introduction/index.html#structure-and-architecture).

One can find out about [other scripts](https://twiki.cern.ch/twiki/bin/view/CMSPublic/SWGuideSkeletonCodeGenerator?rev=15) like mkedanlzr by typing `mked` and hitting the <kbd>Tab</kbd> key:
~~~
mked + Tab
~~~
{: .language-bash}

~~~
mkedanlzr  mkedfltr   mkedlpr    mkedprod
~~~
{: .output}

In this workshop, however, we will not be using those other ones.

There are also additional scripts, like the [EDM utilities](https://twiki.cern.ch/twiki/bin/view/CMSPublic/WorkBookEdmUtilities?rev=21), which can be useful.

> ## Finding the EventSize of a ROOT EDM file
>
> Now, as a simple exercise, use one of the EDM utilities mentioned above to find out about the number of events in the ROOT file that is in the `Demo/DemoAnalyzer/demoanalyzer_cfg.py` config file of your analyzer package.
>
> > ## Solution
> >
> > After checking the documentation above, the following one-liner will work.
> >
> > ~~~
> > edmEventSize -v root://eospublic.cern.ch//eos/opendata/cms/Run2011A/ElectronHad/AOD/12Oct2013-v1/20001/001F9231-F141-E311-8F76-003048F00942.root |grep Events
> > ~~~
> > {: .language-bash}
> >
> > ~~~
> > File root://eospublic.cern.ch//eos/opendata/cms/Run2011A/ElectronHad/AOD/12Oct2013-v1/20001/001F9231-F141-E311-8F76-003048F00942.root Events 16728
> > ~~~
> > {: .output}
> >
> > So the ROOT file has 16728 events.
> >
> {: .solution}
{: .challenge}

{% include links.md %}
