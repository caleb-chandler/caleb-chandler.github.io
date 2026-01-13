---
layout: single
title: "The Transfer Portal: Visualized -- A College Football Network Analysis"
---
# Preface
College football, in my humble opinion, is the greatest sport on Earth. It can be hard to explain to people why the sport is so appealing to me even while I remain lukewarm on the NFL. Sure, on a personal level I was born into it, and I experienced some of my most cherished childhood memories at games, but more than that, there's a powerful, quintessentially American charm to college football that the NFL just doesn't have. The NFL is unapologetically a cynical commercial operation (which I suppose, in its own way, is also quintessentially American), and it receives the best players, but in exchange becomes sterile, corporate, and disengaged from the "common people." 
College is different. The game is older here. It's realer. Rawer. More genuine. It's entangled intimately with identity: religious, regional, cultural, and socioeconomic. Family ties go back generations. You can grow up, as I did, in any far flung forgotten corner of the country, and still have a team to root for. It can be easy to forget that the very birth of football itself was a game between colleges.

But the sport is changing, and rapidly. Players can now be paid not only for their name and likeness (a development that frankly should have come much sooner), but even directly by schools themselves. The playoff was expanded from 4 to 12 teams. Conferences are no longer even nominally regional, but instead --under pressure from TV networks -- made the callous decision to expand wantonly with total disregard for the myriad cultural traditions that made the sport special to begin with, and -- much like the sterilized, corporate NFL -- have thoroughly abandoned all pretense of existing for any other purpose than to maximize profit.

Whether these developments, on balance, are good or bad for the sport depends on who you ask, but there can be no doubt that American capitalism and its ever-insatiable appetite -- dutifully kept at arm's length for so long -- is finally beginning to break down the door. True, there was always money to be made in college football, and certainly quite a lot was even before these changes, but at the same time there was an unspoken understanding that what we had here was not just special, but fragile, and -- like a ___ -- required a certain degree of protection from the outside world if it was to survive. That understanding is no more, and the current direction of the sport is putting that delicate balance on a knife's edge. As Michael MacKelvie puts it in [this fantastic video](), "...".

This is a story about just one of those changes, and perhaps the most polarizing one: **the transfer portal**. 

The transfer portal as it appears today began in earnest on April 28, 2021, when the NCAA ratified a policy change allowing, for the first time, players to leave one team for another without being required to sit out a year in order to do so. While there is broad consensus that the portal has certainly had _an_ effect, there is comparatively very little about what that effect has actually been. Some deride the portal as a ..., while others argue it has actually been beneficial by acting as a ... that allows free circulation of talent and subsequently increases parity.

As for me, I saw it a little bit differently. I am, of course, a major college football fan, but I'm also a data analyst and a student of network science, and if there is one thing that the transfer portal _most definitely_ is, it's a _network_. Given my uniquely relevant background, I would have been remiss if I did not take it upon myself to capitalize on that fact. Using the database and API so generously provided by [CollegeFootballData.com](collegefootballdata.com), I was able to gather all the information I needed, convert it to GraphML format, and -- with only a small amount of annoying cleanup required -- commence, to the best of my ability, with a comprehensive network analysis. What follows is the surprising, and fascinating, result.

# Introduction + A Primer on Network Science

Before we begin, it will be prudent to give a brief rundown of what network science actually is for the uninitiated -- along with the meanings of some of its associated terms.

At its core, network science is the study of _connectivity_. It moves the focus away from the individual characteristics of single entities and toward the relationships _between_ those entities, analyzing the resulting "whole" as one new, consolidated entity: a network, which encodes in its relationships a high-level description of the entire system.

[img]

Networks are applications of [graph theory](https://en.wikipedia.org/wiki/Graph_theory) to the analysis of real-world systems. They are composed of nodes -- individual entities or elements -- and edges, the links between these nodes, which can be weighted by values representing the number of links between the same nodes, the "strength" of the links, or some other attribute. Edges can be either directed (meaning they "point" from one node to another) or undirected. Both nodes and edges can be imbued with any number of attributes. In the parlance of the field, "network" is often used interchangeably with "graph."
Networks can also be bipartite, where nodes are given "classes" and nodes of one class can only connect with nodes of the other (used to model relationships like pollinator-plant, author-paper, etc). Bipartite networks can be "projected" to turn them into a more traditional network isolating the nodes of either class, where edges between nodes in the projected network represent a node of the other class that they both share a link to in the original network (for instance, two genes that share an association with a particular disease).

Network science, then, is a field united much more by methodology than subject matter. It's been used to study all sorts of subjects, ranging from [disease transmission]() to the [global financial system]() to [synaptic connections between neurons in the brain]().

In the context of the transfer portal, the application is rather obvious. Schools are the nodes and transferring players are the directed edges between them. Edges can be weighted either by the number of transfers or the ratings of the transfers. Each year/cycle since 2021 has its own network, and these networks can be compared and aggregated.

[graphml ss]

What's more, even traditional recruiting can be represented as a network: a bipartite one, with schools on one side and recruit hometowns on the other. It can then be projected by school to identify shared recruiting grounds. In this case the usable data goes back much further (specifically to 2002). The recruiting network has some interesting findings, too -- including one especially cool one -- but we can save that for later. For now, we'll just focus on the portal.

# Anatomy of the Transfer Network

## I. Topology (What kind of network is this?)

[img]

Here, at long last, is a full image of the transfer portal in all its glory -- in this case for the most recent year, 2025. Nodes are sized and colored by their degree (number of connections), so that higher-degree schools are larger and bluer. For the sake of readability, I've restricted labels to only the highest-degree schools.
A couple fun things immediately stand out here: for instance, we can observe huge outflows from Marshall to Southern Miss and from South Dakota State to Washington State as a result of players following their coaches to new schools. We also observe that Purdue had an extremely turbulent year in the portal (high degree) following their abysmal 2-10 season and subsequent need to replace departing players. 
But that doesn't tell us anything new. To get to the really juicy insights, we'll need to dig deeper.

The structure of a network is called its [topology](). In nature, there are certain patterns we generally observe in topologies, depending on the context of the network and the underlying process that created it. These patterns can be found by obtaining certain network statistics, such as degree, clustering coefficient (the percentage of a node's neighbors that are also neighbors themselves), density (the ratio of possible edges to existing ones), and so on. For example, when a network is highly clustered and has a short average path-length (technically average _shortest_ path-length) between any two given nodes, we categorize it as "small-world." Small-world topologies are especially common in social networks, where the probability of two contacts being contacts themselves is relatively high. This creates a highly modular system comprised of many distinct clusters (if you've ever read "_The Strength Of Weak Ties_," this is basically what that book is about).

These are some aggregated statistics for our transfer portal networks (calculated as averages of averages so as to avoid privileging years with more data):

| Metric | Value |
| :--- | :--- |
| Average Path Length | 3.046 |
| Diameter | 6.8 |
| Average Degree | 11.17 |
| Average Clustering Coefficient | 0.0585 |
| Density | 0.0175 |

And here is their average degree distribution, which roughly follows a power law with exponent __ (meaning that the probability of engaging in the portal at a certain rate scales up by __ as that rate decreases)

[img]

The transfer portal is notable because its topology actually doesn't fit neatly into any particular “class.” Like a small-world network, it features an extremely short average path-length of about 3 steps between any two programs, but its low density and low clustering coefficient means it cannot form the dense, localized communities that usually define the small-world category.

To find some clues as to why this may be, we can use the [Fruchterman-Reingold](https://karobben.github.io/2023/02/20/LearnNotes/fruchterman-reingold/) algorithm -- one of several "layout algorithms" used to visualize networks. It works by treating nodes as electrically-charged particles, pushing unconnected nodes apart and pulling connected ones together until an energy-minimizing "equilibrium" is reached. Applying this algorithm to our network and coloring the nodes by division (FBS blue, FCS green, lower divisions pink) reveals a major culprit behind its topology:

[fr viz]

There is a clear hierarchy between divisions, with D2 and D3 programs at the outer fringes of the network and FBS programs in the center. This reflects a pattern where teams at higher levels engage in the portal at a significantly higher rate.
[explain how this explains the topology]

As for clustering, it is functionally nonexistent here because players select their next school based on factors that have nothing to do with their previous one (likely NIL, playing time, coach movement, etc.)

## II. Centrality (Which schools are most important?)

In network science, we use a certain class of statistics -- called centrality metrics -- to obtain some measure of a node's "importance" in a network. There are many centrality metrics to choose from, each of which defines importance in its own way. For this network, I have selected two: HITS and PageRank.

[PageRank](https://en.wikipedia.org/wiki/PageRank#Simplified_algorithm) was initially developed as a search ranking algorithm by Larry Page and Sergey Brin and was the impetus for the founding of Google. It's a way of measuring the importance of a node (in their case a webpage) by measuring the importance of its neighbors. It is designed specifically for directed networks, making it a natural choice here.
[HITS](https://en.wikipedia.org/wiki/HITS_algorithm#Algorithm), incidentally, was also originally made to rank webpages. It's a ... that is technically a combination of two metrics: a Hub score and an Authority score. Hub is ... and Authority is ... For our purposes, we're going to ignore the Hub score because ... and just focus on the Authority score, since ...

Here are the distributions of PageRank and HITS Authority scores for all schools, averaged over the years.

![Alt text 1](URL_OF_IMAGE_1) ![Alt text 2](URL_OF_IMAGE_2)

They both fit a power law, indicating that the distribution is highly unequal.

Calculating the centrality values for each node in the aggregated network, we get these results:

[img]

This is a visual representation of the top 15 schools by each metric. Metrics are calculated from the full network but the visualization algorithm was only run on the second (to improve readability). ...

## III. Directionality (Who's gaining and losing?)

...

Since we are dealing with a directed network, we can take advantage of in-degree and out-degree (incoming vs outgoing transfers) to create two simple node-level metrics. We'll call them "Net Portal Gain" and "Net Portal Value," or NPG and NPV. NPG is just the net degree -- the number of in-transfers minus out-transfers for a node -- and NPV is the same but with player rating.
This is the distribution of NPG for all five years.

[img]

When we isolate the top and bottom schools by aggregate (summed) NPG, we get something interesting.

[img]

The bottom 15 is filled almost exclusively with _blue-bloods_, some of the most successful and prestigious programs of the past 25 years. These programs -- Alabama especially -- are being drained of players at an astounding rate. Meanwhile, the other side consists mainly of much-less-prestigious G5 schools which are happily profiting off the difference.

As you may have already guessed, change in NPV is driven almost entirely by change in NPG. This is because the range of ratings is very tight, at 0.75-0.99, meaning most of the difference in raw NPV is being driven by pure volume.

[correlation]

If we control for NPG and isolate the effect of NPV, however, we get this:

[img]

[description of chart]

We can also look at in-degree and out-degree in isolation, allowing us to see who's gaining and losing the most in absolute terms without considering the ratio.

[img]

## IV. Interpretation (What effect has the portal actually had?)

Anecdotally, most of can just kind of "feel" that there's more parity now than there used to be. Indiana's meteoric ascent would not have been possible without the contribution of transfers like Fernando Mendoza and Elijah Sarratt. Similarly, Vanderbilt's sudden and remarkable rise from the depths of mediocrity was spearheaded by quarterback Diego Pavia, another transfer. The last time any of the four teams remaining in the playoff (at the time of this writing) ever won a championship was 2001. So does the data agree with the gut in this case?

The results of our network analysis suggest that there are competing dynamics at play. Top recruits still cluster at a small number of elite programs, but now, with the advent of the portal, these programs can no longer hoard them several layers down on the depth chart. Since these players are still quite good, they are valuable on the market and well able to transfer elsewhere, which cannot necessarily be said for backups at lower-tier programs. Notwithstanding several notable exceptions, transfers are statistically far more likely to be moving _down_ than _up_; it's just that we don't hear about them as often.

[bar plot]

As a result of this dynamic, the pattern we see is that of top programs trading **quantity for quality**, hemorrhaging a massive amount of _volume_ while at the same time bringing in a small quantity of nevertheless elite talent. Nowhere is this pattern illustrated more clearly than in these charts:

[img]

These are Sankey diagrams, made for visualizing flow rates. Used here, they show all the journeys made by transferring players over the past five seasons; first by division, and then by conference. ...

[img]

To top it all off, we can do a quick, straightforward comparison to see directly how parity has changed since 2021. We can't use ratings for this because they remain static unless a player enters the portal, so we'll have to use the next best thing: PPA, or predicted points added, essentially an adaptation of baseball's EPA to a football context.
PPA, unfortunately, can only be calculated for offensive skill players due to the notorious difficulty of quantitatively measuring performance for defensive players and linemen. This requires us to make the assumption that variance in performance (via PPA) for offensive skill players is proportional to variance in some hypothetical measure of "performance" for everyone else, or in other words that variance for skill players is representative of variance for all players. I think this is probably a reasonable assumption.
Using PPA as a proxy for player quality and summing it for each team in each year, we can then take the coefficient of variation (CV) for each year and compare its values before vs. after the introduction of the portal. Doing this, we observe that parity does in fact seem to have increased since 2021.

[img]

While top programs are still getting the very best -- and indeed may be getting even higher _top-end_ talent -- the difference in talent between the players they're losing and the players they're gaining is not enough to make up for the loss in volume. These programs always had the best players, but now their overall _share_ of the best players is smaller.
Combined with the status of "mid-tier" programs as go-betweens for rising stars in lower divisions and FCS, the net effect of the portal seems to indeed be an increase in parity.

# The Recruiting Network: Visualizing Pipelines

I promised I'd get to recruiting, so here we are. I must admit that I didn't end up getting to do nearly as much with this as I would have liked, but I was still able to obtain at least one very interesting result that, despite being secondary to the main project, I think is easily the most exciting one I got.
As a reminder, the recruiting networks consist of teams that recruited from the same locations in the same year, with edges weighted by the number of times they did so.

This one turned out to be more of a small-world topology, with high clustering and a short path length. Here are its aggregated statistics and degree distribution, which is best approximated by a one-tailed bell curve (with a predictable slight jump at 0 for recruits from remote areas). This distribution likely just reflects the fact that teams have a limited number of scholarship offers they can make per season.



[img]

Since this is a network with high clustering, we can do something really cool: _community detection_. Community detection is the practice of finding groups of nodes that tend to cluster together in relatively siloed-off groups. The majority of algorithms used for this purpose work by maximizing, in some way or another, a metric called "modularity," which is essentially the difference between the number of edges you observe within a node grouping and the number you would expect to observe if the network were generated randomly.
The [Louvain algorithm]() is one such method, and the one we will use here. It works by grouping nodes together until it can no longer increase the grouping's modularity score, then combining those groupings into one entity, treating them as "nodes," and applying the same process, alternating between stages until no further change can increase modularity. Applying it to our averaged network and again visualizing it with the Fruchterman Reingold algorithm, these are the communities that emerge:

[img]

See anything? Let's try adding labels to the communities:

[img]

What you're looking at is the physical manifestation of recruiting pipelines in "graph space," drawn from real-world data stretching back 24 years.
Not only do we observe four clear partitions, we can easily see -- thanks to our visualization algorithm -- that these four communities are in fact subregions of two highly-distinct pipelines, which happen to correspond almost perfectly with the regions East and West of the Mississippi River.