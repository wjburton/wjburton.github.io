---
layout: post
title:  "Movie Cast/Crew Network Visualization"
date:   2017-10-08 16:07:16 -0700
categories: jekyll update
---

### Intro
This project was supposed to serve as an introduction to Network Analysis and R shiny. The idea was to visualize networks of the cast and crews for specific movie genres and to answer a few questions: Who was the most popular? What are the differences across genres? What are the differences betweeen cast and crew networks?

In the app, each node in the network represents a cast / crew member (depending on what is selected) and a connection  exists if the connected nodes worked on the same movie.

Before answering the initial questions and walking through how it was created, here is the final app:
<iframe src="https://wburton.shinyapps.io/shiny/" style="border: none; width: 1000px; height: 700px"></iframe>

<br>
### Data Pull and Manipulation
To build the visualization, data has to be collected, and it needs to be properly formatted. On this project's github [repo]({{"https://github.com/wjburton/Movie_Network"}}) there is a package built to query TMDB's (the movie database) API, called MovieNetwork.  

When someone clicks submit on the shiny app they are submitting an API request to Themoviedb.org. Based on the given inputs, the first step returns the top n most popular movies. 
!['Movie Pull']({{"/assets/movie_pull.png" | absolute_url}})  
  
Once the most popular movies have been pulled, it performs another api request that pulls the cast and crew information for each movie id in the movie_df.
!['Cast and Crew Pull']({{"/assets/cast_df.png" | absolute_url}})  
  
Now that the data is collected, in order to create a network diagram all that is needed is to format the data into a node-link format. In this case a node is a cast member or crew member (depending on the input selection), and a connection is built if they worked on the same movie. After manipulation, the final format looks like:  
!['Link dataframe']({{"/assets/links.png" | absolute_url}})  
For Example: Javier Bardem is the source node id 0 for lines 1-7. He was in a movie with Jennifer Aniston, Kristin Wiig, etc. It is labeled source and target in the event this were to be a directional graph.

And lastly, the node labels are created:
!['Link dataframe']({{"/assets/node_labels.png" | absolute_url}})
<br>
<br>

### Network Degree Calculations
To help bring forth insights from the graphs created, a table is created (located in the 'Most Influential' tab) that contains normalized degree centrality, betweenness centrality, and eigen centrality. Called 'centrality' because they are trying to point out the most important nodes in a network, depending on what you are calling important.  
**Degree Centrality** indicates general popularity - total number of connections a node has  
**Betweenness Centrality** indicates how much a node helps bridge connections between other nodes. It is equal to the number of shortest paths from all nodes to all others that pass through that node.  
**Eigen Centrality** Indicates if the node is connected to other popular nodes (ie connected to nodes that have many connections).  


Now to answer the initial questions using the app:  
**A. Who is the most popular cast and crew member?**  
This isn't really a straight forward question as it's highly dependent on time and genre, plus you could use a slew of popularity metrics.. but to keep it simple we'll say movies tagged with 'Comedy' from the last 5 years (2012-11-01:2017-11-01), and for a popularity metric I use a normalized degree (number of connections/total possible number of connections). The Number of movies was set 200 for this network to try to be as accurate as possible. Using 200 is not recommended with the embedded ap on the page, it will break if the network requested to build becomes too large.  
The resulting top cast is shown in the table below, ordered by degree centrality:
!['Cast table']({{"/assets/Cast_table2.png" | absolute_url}})
<br>
It's interesting that Randall Park was number 1, But he took on more minor roles than most of the other cast members on the list. 

And corresponding table of the most popular crew members:
!['Crew table']({{"/assets/crew_table.png" | absolute_url}})

**B. What are the differences between genre networks?**  
Comparisons between genres are different depending on whether you are focusing on the cast or crew networks.
- Action has the densest networks across both cast and crews.
- Comedy and fantasy have a slightly lower density than action.
- Animation has more segmented crew networks based on the studio producing the movie. 
- Documentary networks are highly disconnected (as expected). Horror is similar to Romance where the cast has a low level of connectedness.



**C. What are the differences between cast and crew networks?**  
By playing around with the app, you can begin to identify patterns across genres by how tight and connnected the networks are. In addition, the cast and crew networks can be drastically different for a certain genre. To point out some interesting differences:
- Animation movies have a clustered crew network, but a highly connected cast network
- Action is Dense for both cast and crew, and is basically ran by Stan Lee. He is at the center of the densest portion in the crew network.
- Horror movies have some strong crew clusters, but the cast is highly disparate.




