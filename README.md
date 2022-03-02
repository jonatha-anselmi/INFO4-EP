# INFO4-EP: Évaluation de Performances

Welcome to the course "Évaluation de Performances" edition 2021/2022!

[Jonatha Anselmi](mailto:jonatha.anselmi@inria.fr) is in charge of the
lectures while [Louis-Sebastien Rebuffi](mailto:louis-sebastien.rebuffi@univ-grenoble-alpes.fr) is in
charge of practical sessions.

## Course content and planning

| Semaine    | Cours (Jeudi, 9h45-11h15)                                                | TD (Vendredi, 11h30-13h00)                                                                |
|:-----------|:--------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------|
| 10-14 jan. | [Course content and introduction to queueing theory](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_01_intro.pdf)     |  [Discrete Event Simulation of a G/G/1 queue](#practical-session-1)
| 17-21 jan. | [The GI/GI/1 queue](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/EP-Chap2-Bases.pdf)   | Pas de TD                                                                         
| 24-28 jan. | [Little's law](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/EP-Chap2-Bases.pdf) and  [Discrete Time Markov Chains I](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_CMTD.pdf) | Exercises on DTMC
| 01-04 feb. | [Discrete Time Markov Chains II](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_CMTD.pdf)  | [LSR] [Cache](https://github.com/jonatha-anselmi/INFO4-EP/files/6263531/TD7-Memoire-Paginee.pdf) et [Corrigé](https://github.com/jonatha-anselmi/INFO4-EP/files/6263518/Corrige.TD.Pagination.pdf)
| 07-11 feb. | [Poisson Process](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_CMTC.pdf)   | [LSR] [Aloha](https://github.com/jonatha-anselmi/INFO4-EP/files/6263537/TD8-Aloha.pdf)
| 14-18 feb. | [Continuous Time Markov Chains I](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_CMTC.pdf)   | [Presentation of DM + Exercises CTMC](#26-02-2022-practical-session-5)
| 28-04 feb. | [Continuous Time Markov Chains II](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_CMTC.pdf)   | [LSR] Aloha (end) / [Correction](https://github.com/jonatha-anselmi/INFO4-EP/files/6263539/Aloha.elements.de.corection.pdf)
| 07-11 mar. | [Classic Queues](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_CMTC.pdf)   | [Simulation of the M/M/K/K queue in R](practical-session-7)
| 14-18 mar.| The M/G/1 queue   | [LSR] [TD Page Rank.pdf](https://github.com/jonatha-anselmi/INFO4-EP/files/6263543/TD.Page.Rank.pdf) et [Correction](https://github.com/jonatha-anselmi/INFO4-EP/files/6263545/Elements.de.correction.Page.Rank.pdf)
| 21-25 mar. | [Jackson Queueing Networks](#25-03-2022-lecture-10)   | [Performance Evaluation and Optimization of a Web Application](#26-03-2022-practical-session-9)
| 28-01 mar | Pas de cours   | [Quick](#02-04-2022-practical-session-10)
                                                                                  


# Supplementary Material



## Install R and Rstudio
If you're running a debian or an ubuntu,
simply follow the following steps (otherwise, e.g., MacOS X or
Windows, you may want to have a look at [these
guidelines](https://gitlab.inria.fr/learninglab/mooc-rr/mooc-rr-ressources/-/blob/master/module2/ressources/rstudio_fr.org)):

``` shell
sudo apt-get install r-base r-cran-knitr r-cran-tidyverse
```
	
Installing software through your OS package manager is generally
the preferred way to do, although packages can also be installed
from R itself. Make sure you have a recent (>= 3.2.0) version or R. For example,
here is what I have on my machine:
	
``` shell	
R --version
```

    R version 3.5.2 (2018-12-20) -- "Eggshell Igloo"
    Copyright (C) 2018 The R Foundation for Statistical Computing
    Platform: x86_64-pc-linux-gnu (64-bit)

    R is free software and comes with ABSOLUTELY NO WARRANTY.
    You are welcome to redistribute it under the terms of the
    GNU General Public License versions 2 or 3.
    For more information about these matters see
    http://www.gnu.org/licenses/.

If it's not the case, it may be because you're running an old debian
stable or an old LTD ubuntu. In such case, you may want to [include
testing
packages](http://serverfault.com/questions/22414/how-can-i-run-debian-stable-but-install-some-packages-from-testing)... Ask
your local linux guru or run a VM if you're affraid to break your
OS. For the braves, let's keep going!

Rstudio and knitr are unfortunately not packaged within debian so
the easiest is to download the corresponding debian package on the
[Rstudio webpage](http://www.rstudio.com/ide/download/desktop)
and then to install it manually (depending on when you do this,
you should obviously change the version number and you may have to
update the url so that it matches your OS).

``` shell
cd /tmp/
wget https://download1.rstudio.org/desktop/bionic/amd64/rstudio-1.3.1073-amd64.deb
sudo dpkg -i rstudio-1.3.1073-amd64.deb
sudo apt-get update ; sudo apt-get -f install # to fix possibly missing dependencies
```

You may have trouble when installing some R packages. If so, try to
install these ones:

``` shell
sudo apt-get install libcurl4-openssl-dev libssl-dev
```

Finally you should be able to open rstudio. Try then to open a new
RMarkdown document (in the menu File New File R Markdown. When
doing so and depending on what has been installed on your machine,
Rstudio may complain that it requires upgraded versions of knitr,
rmarkdown and tinytex... :( Just proceed and you'll be ready for the
practical session.

## Practical Session 1

The objective is to write together a code in R that simulates the dynamics of a G/G/1 queue and then to use this code to estimate performance metrics such as the average time spent in the system by jobs as a function of the input parameters (e.g., job arrival and service rates). <ins>Make sure to have installed R and Rstudio on your machine</ins> (see above).


### Modeling and principle of simulation

#### System parameters

- Job arrival rate `lambda` (job/sec)
- Service rate `mu` (job/sec)
- Scheduling discipline (we will use FIFO but we will keep the code as general as possible)
- Number of jobs to simulate `N`

#### Description of state variables:

- Current time `t`
- Arrival times `Arrival`: computed by job interarrival times (input)
- Service times `Service` (input)
- Residual service times `Remaining`: initialized with `NA`, set to `Service` when a job arrives and decreased to zero.
- Completion times `Completion`: initialized with `NA`, set to the current time `t` when `Remaining` becomes zero.
- Job in execution `CurrClient`: initialized with `NA`

The variable `NextArrival` may be convenient to use to get a simpler code. This variable is initially set to one and increased by one without exceeding the overall number of jobs to simulate `N`.

#### System evolution

Either an arrival or a service completion occurs

#### Code structure

``` R
while(T) {
    dtA = ...  # temps jusqu'à la prochaine arrivée
    dtC = ...  # temps jusqu'à la prochaine terminaison
    if(is.na(dtA) & is.na(dtC)) {break;}
    dt = min(dtA,dtC)
    # Mettre à jour comme il faut.
}
```

#### Objective
- To have a working code
- To plot the average response time as a function of the arrival rate `lambda`. Here, we assume that service times are exponentially distributed with rate one and that interarrival times are exponentially distributed with rate `lambda`. Also, `N=10000`.


#### Code

Link to [code](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/GG1.Rmd). Plotting the mean response time as a function of `lambda`, what conclusion can you make?




## Practical Session 7

The student knows how to simulate classic queueing systems in R.

[Simulation code for the M/M/K/K queue](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/MMKK.Rmd), and [[link on rpubs](https://rpubs.com/janselmi/MMKK)].




Jackson queueing networks; identification of the underlying continuous time Markov chain; global balance equations; Jackson's theorem and product form; and computation of performance measures of interest such as the overall mean response time
[[slides](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/RICM4_EP_FA.pdf)].


## [26-03-2022] Practical Session 9

Performance Evaluation and Optimization of a web application; theory and simulation; all the details on [[rpubs](https://rpubs.com/janselmi/webapp)].

## [02-04-2022] Practical Session 10

Correction to [Quick](https://github.com/jonatha-anselmi/INFO4-EP/blob/main/quick_2020_Exam_18.pdf)

