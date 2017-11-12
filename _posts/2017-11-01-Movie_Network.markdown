---
layout: post
title:  "Movie Cast/Crew Network Visualization"
date:   2017-10-08 16:07:16 -0700
categories: jekyll update
---

### Intro
This project was supposed to serve as an introduction to Network Analysis and R shiny. The idea was to visualize networks of the Cast and Crews for specific movie Genres and to answer a few questions: Who was the most popular? Who likes to work togethor? What are the differences across genres? What are the differences betweeen cast and crew networks?


Before answering these questions we had to set up the data. On my github repo for this project there is a package we built to query TMDB's (the movie database) API, called MovieNetwork.
<br>
<br>  
### Data Pull and Manipulation

When someone clicks submit on the shiny app they are submitting an API request to Themoviedb.org. Based on the given inputs, the first step returns the top n most popular movies. 
!['Movie Pull']({{"/assets/movie_pull.png" | absolute_url}})  
  
Once the most popular movies have been pulled, it performs another api request that pulls the cast and crew information for each movie id in the movie_df.
!['Cast and Crew Pull']({{"/assets/cast_df.png" | absolute_url}})  
  
Now that the data is collected, in order to create a network diagram all we need to do is format the data into a node-link format. In this case a node is a cast member or crew member (depending on the input selection), and a connection is built if they worked on the same movie. After manipulation, the final format looks like:  
!['Link dataframe']({{"/assets/links.png" | absolute_url}})  
For Example: Javier Bardem is the source node id 0 for lines 1-7. He was in a movie with Jennifer Aniston, Kristin Wiig, etc. It is labeled source and target in the event this were to be a directional graph.

And lastly, the node labels are created:
!['Link dataframe']({{"/assets/node_labels.png" | absolute_url}})
<br>
<br>

### Network Degree Calculations
To help bring forth insights from the graphs created, we also make a table located in the 'Most Influential' tab on the app. It contains normalized degree centrality, betweenness centrality, and eigen centrality. Called centrality because they are trying to point out the most important nodes in a network, depending on what you are calling important.  
**Degree Centrality** indicates general popularity - total number of connections a node has  
**Betweenness Centrality** indicates how much a node helps bridge connections between other nodes. It is equal to the number of shortest paths from all nodes to all others that pass through that node.  
**Eigen Centrality** Indicates if the node is connected to other popular nodes (ie connected to nodes that have many connections).  


Now to answer the initial question using the app:  
1. Who is the most popular cast and crew member?  
  This isn't really a straight forward question as it's highly dependent on time and genre, plus you could use a slew of popularity metrics.. but to keep it simple we'll say movies tagged with 'Comedy' from the last 5 years (2012-11-01:2017-11-01), and for a popularity metric I use a normalized degree (number of connections/total possible number of connections). The Number of movies was set 200 for this network to try to be as accurate as possible. Using 200 is not recommended with the embedded ap on the page, it will break if the network requested to build becomes too large.  
The resulting top cast is shown in the table below, ordered by degree centrality:
!['Cast table']({{"/assets/Cast_table2.png" | absolute_url}})
<br>
It's interesting that Randall Park was number 1, But he took on more minor roles than most of the other cast members on the list. 


And corresponding table of the most popular crew members:
!['Creq table']({{"/assets/crew_table.png" | absolute_url}})


Instead of finishing off the remainder of the questions, I'll stop here and let anyone interested play around with the app for themselves. 

<iframe src="https://wburton.shinyapps.io/shiny/" style="border: none; width: 1000px; height: 700px"></iframe>

The code for the shiny application, along with the data pull and manipulation package can be found on github [here]({{"https://github.com/wjburton/Movie_Network"}})

