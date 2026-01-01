---
layout: single
title: "The Transfer Portal: Visualized -- A College Football Network Analysis"
---
# Preface
College football, in my humble opinion, is the greatest sport on Earth.

It can be hard to explain to people why the sport is so appealing to me even while I remain lukewarm on the NFL. Sure, on a personal level I was born into it, and I experienced some of my most cherished childhood memories at games, but more than that, there's a powerful, quintessentially American charm to college football that the NFL just doesn't have. The NFL is unapologetically a cynical commercial operation (which I suppose, in its own way, is also quintessentially American), and it receives the best players, but in exchange becomes sterile, corporate, and disengaged from the "common people." 
College is different. The game is older here. It's realer. Rawer. More genuine. It's entangled intimately with identity: religious, regional, cultural, and socioeconomic. Family ties go back generations. You can grow up, as I did, in any far flung forgotten corner of the country, and still have a team to root for. It can be easy to forget that the very birth of football itself was a game between colleges.

But the sport is changing, and rapidly. Players can now be paid not only for their name and likeness (a development that frankly should have come much sooner), but even directly by schools themselves. Conferences are no longer even nominally regional, but have instead made the callous decision to expand wantonly with total disregard for the myriad cultural traditions that made the sport special to begin with, and -- much like the sterilized, corporate NFL -- have thoroughly abandoned all pretense of existing for any other purpose than to maximize profit (it is my opinion that independent conferences should be done away with altogether, but that's a whole other discussion).

Whether these developments, on balance, are good or bad for the sport depends on who you ask, but there can be no doubt that American capitalism and its ever-insatiable appetite -- dutifully kept at arm's length for so long -- is finally beginning to break down the door. True, there was always money to be made in college football, and certainly quite a lot was even before these changes, but at the same time there was an unspoken understanding that what we had here was not just special, but fragile, and -- like a ___ -- required a certain degree of protection from the outside world if it was to survive. That understanding is no more, and the current direction of the sport is putting that delicate balance on a knife's edge. As Michael MacKelvie puts it in [this fantastic video](), "____".

This is a story about just one of those changes, and perhaps the most polarizing one: **the transfer portal**. 

The transfer portal in all its glory began in earnest on April 28, 2021, when the NCAA ratified a policy change allowing, for the first time, players to leave one team for another without being required to sit out a year in order to do so. While there is broad consensus that the portal has certainly had _an_ effect, there is comparatively very little about what that effect has actually been. Some deride the portal as a ___, while others argue it has actually been beneficial by acting as a ___ that allows free circulation of talent and subsequently increases parity.

As for me, I saw it a little bit differently. I am, of course, a major college football fan, but I'm also a data analyst and a student of network science, and if there is one thing that the transfer portal _most definitely_ is, it's a _network_. Given my uniquely relevant background, I would have been remiss if I did not take it upon myself to capitalize on that fact. Using the database and API so generously provided by the people over at [CollegeFootballData.com](collegefootballdata.com), I was able to gather all the information I needed, convert it to GraphML format, and -- with only a small amount of annoying cleanup required -- commence, to the best of my ability, with a comprehensive network analysis. What follows is the surprising, and fascinating, result.

# Introduction ("What is a network anyway?")

Before we begin, it will be prudent to explain what network science actually is for the uninitiated -- along with the meanings of some of its associated terms.

At its core, network science is the study of _connectivity_. It moves the focus away from the individual characteristics of single entities and toward the relationships _between_ those entities, analyzing the resulting "whole" as a new, consolidated entity: a network, which encodes in its relationships a coarse-grained description of the entire system.

[img]

Networks are applications of graph theory (if you know what that is) to the analysis of real-world systems. They are composed of nodes -- individual entities or elements -- and edges, the links between these nodes, which can be weighted by values representing the number of links between the same nodes, the "strength" of the links, or some other attribute. Edges can be either directed (meaning they "point" from one node to another) or undirected. Both nodes and edges can be imbued with any number of attributes. 
Networks can also be bipartite, where nodes are given "classes," and nodes from one class can only connect with nodes of the other (used to model relationships like pollinator-plant, author-paper, etc). Bipartite networks can be "projected" to turn them into a more traditional network isolating the nodes of either class, where edges in the projected network represent an edge two nodes of the same class share with a node of the other in the original network (for instance, two genes that share an association with a particular disease).

Network science, then, is a field united much more by methodology than subject matter. It's been used to study subjects ranging from [disease transmission]() to the [global financial system]() to [synaptic connections between neurons in the brain]().

In the context of the transfer portal, the application is rather obvious. Schools are the nodes and transferring players are the directed edges between them. Edges can be weighted either by the number of transfers or the ratings of the transfers. Each year/cycle since 2021 has its own network, and these networks can be compared and aggregated.

[img]

What's more, even traditional recruiting can be represented as a network: a bipartite one, with schools on one side and recruit hometowns on the other. It can then be projected by school to identify shared recruiting grounds. In this case the usable data goes back much further, specifically to 2002.

[img]

The recruiting network has some interesting findings, too (including one especially cool one), but we can save that for later. For now, we'll just focus on the portal.

# Part I: Anatomy of the Network



Taken together, the evidence tells a very particular story. 

Top recruits still cluster at a small number of elite programs, but now, with the advent of the portal, elite programs can no longer hoard them several layers down on the depth chart. Since these players are still quite good, they are valuable on the market and well able to transfer elsewhere, which the same cannot be said for backups at lower-tier programs. Notwithstanding several notable exceptions, transfers are statistically far more likely to be moving _down_ than _up_.

As a result of this dynamic, the pattern we see is that of top programs trading **quantity for quality**, hemorrhaging a massive amount of _volume_ while at the same time bringing in a small quantity of nevertheless elite talent. Nowhere is this dynamic illustrated more clearly than in [description of chart]:

[img]

But what about parity? Anecdotally, most of can just kind of "feel" that there's more parity now than there used to be. Indiana's meteoric ascent would not have been possible without transfers like Fernando Mendoza and Elijah Sarratt. Similarly, Vanderbilt's sudden and remarkable rise from the depths of mediocrity was spearheaded by star quarterback Diego Pavia, another transfer. So does the data agree with the gut in this case?

As it turns out, the truth is more nuanced. 