# Parallel kmeans scripts
<div style="text-align: justify">R is a powerful scripting language for mapping and data visualization, which can execute a large number of mathematical models and algorithms. However, due to its low system execution efficiency, it will be difficult to deal with the problem of large amount of data. Here is a case of parallel kmeans clustering for everyone to learn. </div>
<div style="text-align: justify"> <br> </div>
<div style="text-align: justify">For the parallel running script of kmean mean value, the best of the 10 running results is better selected. After testing, it does not affect the operation results, greatly speeds up the operation speed and saves the script operation time. It is recommended to use when calculating large data sets.  </div>
<div style="text-align: justify"> <br> </div>
<details>
<summary> Click to view code </summary>
<pre><code>
#####By Sean from MUST#####
#Induce parallel package
library(parallel)

#Define number of cpu core (default: total -2)
nw <- detectCores()-2
cl <- makeCluster(nw)

#Define nstart 
nstart <- 10
nstartv <- rep(ceiling(nstart / nw), nw)

#read lig_noh_pos.txt
data <- read.table("data.txt")

#run clusterApply
data_km <- clusterApply(cl, nstartv,
        function(n, x) kmeans(x, 1000, nstart=n, iter.max=100),
        data)
        
#Pick the best result
i <- sapply(data_km , function(data_km) data_km $tot.withinss)
data_km  <- data_km [[which.min(i)]]
print(data_km$tot.withinss)
per_atom_rmsd<-sqrt((data_km$withinss/(data_km$size-1))/2914) 
summary(data_km$size)
summary(per_atom_rmsd)
</code></pre>
</details>
