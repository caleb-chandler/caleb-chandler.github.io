---
layout: single
title: "The Transfer Portal: Visualized -- A College Football Network Analysis"
---
# Preface
College football, in my humble opinion, is the greatest sport on Earth. It can be hard to explain to people why the sport is so appealing to me even while I remain lukewarm on the NFL. Sure, on a personal level I was born into it, and I experienced some of my most cherished childhood memories at games, but more than that, there's a powerful, quintessentially American charm to college football that the NFL just doesn't have. The NFL is unapologetically a cynical commercial operation (which I suppose, in its own way, is also quintessentially American), and it receives the best players, but in exchange becomes sterile, corporate, and disengaged from the "common people." 
College is different. The game is older here. It's realer. Rawer. More genuine. It's entangled intimately with identity: religious, regional, cultural, and socioeconomic. Family ties go back generations. You can grow up, as I did, in any far flung forgotten corner of the country, and still have a team to root for. It can be easy to forget that the very birth of football itself was a game between colleges.

But the sport is changing, and rapidly. Players can now be paid not only for their name and likeness (a development that frankly should have come much sooner), but even directly by schools themselves. The playoff was expanded from 4 to 12 teams. Conferences are no longer even nominally regional, but have instead made the callous decision to expand wantonly with total disregard for the myriad cultural traditions that made the sport special to begin with, and -- much like the sterilized, corporate NFL -- have thoroughly abandoned all pretense of existing for any other purpose than to maximize profit.

Whether these developments, on balance, are good or bad for the sport depends on who you ask, but there can be no doubt that American capitalism and its ever-insatiable appetite -- dutifully kept at arm's length for so long -- is finally beginning to break down the door. True, there was always money to be made in college football, and certainly quite a lot was even before these changes, but at the same time there was an unspoken understanding that what we had here was not just special, but fragile, and -- like a ___ -- required a certain degree of protection from the outside world if it was to survive. That understanding is no more, and the current direction of the sport is putting that delicate balance on a knife's edge. As Michael MacKelvie puts it in [this fantastic video](), "____".

This is a story about just one of those changes, and perhaps the most polarizing one: **the transfer portal**. 

The transfer portal as it appears today began in earnest on April 28, 2021, when the NCAA ratified a policy change allowing, for the first time, players to leave one team for another without being required to sit out a year in order to do so. While there is broad consensus that the portal has certainly had _an_ effect, there is comparatively very little about what that effect has actually been. Some deride the portal as a ___, while others argue it has actually been beneficial by acting as a ___ that allows free circulation of talent and subsequently increases parity.

As for me, I saw it a little bit differently. I am, of course, a major college football fan, but I'm also a data analyst and a student of network science, and if there is one thing that the transfer portal _most definitely_ is, it's a _network_. Given my uniquely relevant background, I would have been remiss if I did not take it upon myself to capitalize on that fact. Using the database and API so generously provided by [CollegeFootballData.com](collegefootballdata.com), I was able to gather all the information I needed, convert it to GraphML format, and -- with only a small amount of annoying cleanup required -- commence, to the best of my ability, with a comprehensive network analysis. What follows is the surprising, and fascinating, result.

# Introduction

Before we begin, it will be prudent to explain what network science actually is for the uninitiated -- along with the meanings of some of its associated terms.

At its core, network science is the study of _connectivity_. It moves the focus away from the individual characteristics of single entities and toward the relationships _between_ those entities, analyzing the resulting "whole" as a new, consolidated entity: a network, which encodes in its relationships a coarse-grained description of the entire system.

[img]

Networks are applications of graph theory (if you know what that is) to the analysis of real-world systems. They are composed of nodes -- individual entities or elements -- and edges, the links between these nodes, which can be weighted by values representing the number of links between the same nodes, the "strength" of the links, or some other attribute. Edges can be either directed (meaning they "point" from one node to another) or undirected. Both nodes and edges can be imbued with any number of attributes. 
Networks can also be bipartite, where nodes are given "classes" and nodes of one class can only connect with nodes of the other (used to model relationships like pollinator-plant, author-paper, etc). Bipartite networks can be "projected" to turn them into a more traditional network isolating the nodes of either class, where edges in the projected network represent an edge two nodes of the same class share with a node of the other in the original network (for instance, two genes that share an association with a particular disease).

Network science, then, is a field united much more by methodology than subject matter. It's been used to study all sorts of subjects, ranging from [disease transmission]() to the [global financial system]() to [synaptic connections between neurons in the brain]().

In the context of the transfer portal, the application is rather obvious. Schools are the nodes and transferring players are the directed edges between them. Edges can be weighted either by the number of transfers or the ratings of the transfers. Each year/cycle since 2021 has its own network, and these networks can be compared and aggregated.

[img]

What's more, even traditional recruiting can be represented as a network: a bipartite one, with schools on one side and recruit hometowns on the other. It can then be projected by school to identify shared recruiting grounds. In this case the usable data goes back much further (specifically to 2002). The recruiting network has some interesting findings, too -- including one especially cool one -- but we can save that for later. For now, we'll just focus on the portal.

# Anatomy of the Network

## I. Topology (What kind of network is this?)

[img]

Here, at long last, is a full image of the transfer portal in all its glory -- in this case for the most recent year, 2025. ...

The structure of a network is called its [topology](). In nature, there are certain patterns we generally observe in topologies, depending on the context of the network and the underlying process that created it. These patterns can be found by aggregating certain network statistics, such as clustering coefficient (the percentage of a node's neighbors that are also neighbors themselves), density (the ratio of possible edges to existing ones), degree (the total number of neighbors), and so on. For example, when a network is highly clustered, has a short average path-length (technically average _shortest_ path-length) between any two given nodes, and [has] a heavy-tailed degree distribution, we categorize it as "small-world." Small-world topologies are especially common in social networks, where the probability of two contacts being contacts themselves is relatively high due to the nature of socialization (if you've ever read "_The Strength Of Weak Ties_," this is basically what that book is about).

The transfer portal is notable because its topology actually doesn't fit neatly into any particular “class.” Like a small-world network, it exhibits a very short average path length and a relatively small [diameter](), meaning that nearly any two programs are only a few steps apart through chains of transfers. But it is also extremely sparse, with low density (0.02) and a low average clustering coefficient (0.092). It's also highly asymmetric, with connectivity dominated by directional flows rather than dense, reciprocal neighborhoods. Degree varies substantially across nodes, but this is due to hierarchical differentiation rather than the triadic closure typical of social networks. In other words, the transfer portal combines the global reachability of a small-world social network with the sparse, hub-mediated structure of a flow or infrastructure network.

To find some clues as to why this may be, we can use the Fruchterman-Reingold algorithm -- one of several "layout algorithms" used to visualize networks. It works by treating nodes as if they were electrically-charged particles, pushing unconnected nodes apart and pulling connected ones together to find an "entropy-maximizing" equilibrium. Applying this algorithm to our network and coloring the nodes by division reveals a major culprit behind its topology:

[fr viz]

There is a clear hierarchy between divisions, with D2 and D3 programs at the outer fringes of the network and FBS programs in the center. This reflects a classic hierarchical "out-to-in" flow pattern, where significantly more players transfer up from lower divisions than down to them. (this notably does _not_ hold _within_ divisions, however; we'll explore that later). This pattern makes sense given that there is usually little reason for a player in D2 or D3 to transfer laterally.

Clustering is functionally nonexistent here because players select their next school based on factors that have nothing to do with their previous one. Destinations must be driven primarily by factors not included in my data, such as NIL, playing time, coach movement, etc.

## II. Directionality (Where are players going?)

...

Since we are dealing with a directed network, we can take advantage of in-degree and out-degree (incoming vs outgoing transfers) to create two simple node-level metrics. We'll call them "Net Portal Gain" and "Net Portal Value," or NPG and NPV. NPG is just the net degree -- the sum of in-transfers and out-transfers for a node -- and NPV is the same but with player rating.

This is the distribution of NPG for all five years.

[img]

When we isolate the top and bottom schools by aggregate (summed) NPG, we see something interesting.

[img]

The bottom 10 is filled almost exclusively with _blue-bloods_, some of the very most successful and prestigious programs of the past 25 years. These programs all seem to be hemorrhaging players at an astounding rate. Meanwhile, the other side consists mostly of much-less-prestigious G5 schools which are soaking up the difference.
This is interesting because it suggests ...

As you may have already guessed, change in NPV is driven almost entirely by change in NPG; a correlation made even stronger by the fact that no players are rated lower than 0.75.

[correlation]

If we control for NPG and isolate the effect of NPV, however, we get this:

[img]

[description of chart]

...

## III. Centrality (Which schools are most important?)

In network science, we use a certain class of metrics -- called centrality metrics -- to obtain some measure of a node's "importance" in a network. There are many of these metrics to choose from, each of which defines importance in its own way. For our purposes, I have selected three: betweenness, closeness, and PageRank.

Betweenness centrality works by finding the shortest paths between all pairs of nodes in the network and then finding the percentage of these paths that run through a particular node. It is designed to measure the extent to which a certain node acts as a "middleman" between disparate parts of the network.
Closeness centrality is ...
PageRank was initially developed by Larry Page and Sergey Brin and is the algorithm they used to found Google. It's a way of measuring the importance of a node by measuring the importance of its neighbors. ... It was designed specifically for directed networks, making it a natural choice.



# Meaning & Interpretation

Anecdotally, most of can just kind of "feel" that there's more parity now than there used to be. Indiana's meteoric ascent would not have been possible without transfers like Fernando Mendoza and Elijah Sarratt. Similarly, Vanderbilt's sudden and remarkable rise from the depths of mediocrity was spearheaded by quarterback Diego Pavia, another transfer. The last time any of the four teams remaining in the playoff (at the time of this writing) has ever won a championship was 2001. So does the data agree with the gut in this case? And if so, why?

The analysis suggests that there are competing dynamics at play. Top recruits still cluster at a small number of elite programs, but now, with the advent of the portal, those programs can no longer hoard them several layers down on the depth chart. Since these players are still quite good, they are valuable on the market and well able to transfer elsewhere, which cannot necessarily be said for backups at lower-tier programs. Notwithstanding several notable exceptions, transfers are statistically far more likely to be moving _down_ from the P4 than _up_ from the G6.
As a result of this dynamic, the pattern we see is that of top programs trading **quantity for quality**, hemorrhaging a massive amount of _volume_ while at the same time bringing in a small quantity of nevertheless elite talent. Nowhere is this pattern illustrated more clearly than in this chart:

[img]

This is a Sankey diagram of conference affiliation, showing where players move to based on where they started. ...

While top programs are still getting the very best -- and indeed may be getting even higher _top-end_ talent -- the difference in talent between the players they're losing and the players they're gaining is not enough to make up for the loss in volume. These programs always had the best players, but now their overall _share_ of the best players is smaller.
Combined with the status of "mid-tier" programs as go-betweens for rising stars in lower divisions and FCS, the net effect of the portal is, indeed, to increase parity.

# Bonus: Recruiting and Talent Metrics

