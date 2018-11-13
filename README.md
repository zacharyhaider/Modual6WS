# 44-517 Workshop 8 [Tu-Nov-13] - Spark, RDD
## Required Software
* [Oracle JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Scala](https://www.scala-lang.org/download/)
* [Spark](https://spark.apache.org/downloads.html)
* Git bash (if useing Windows)
* A text editor of your choice(optional but handy)

## Group Members
* Luke Carlson
* Anik Paul Gomes
* Zachary Haider
* Goutham Neravetla

## Our cenario: 
You are a computer scientist by day and a thespian by night. The theater you work for decides that everyone gets to pick Shakespearean name and will be that character in every play regardless of the significance. Ofelia, the star of Twelfth Night and crazy victim in Hamlet. All you care about however, is the number of lines you can read. More lines = longer on stage =  more time to be discovered. Here is what you do.

## What we wish we knew:



## Our code:

### Step one:
First download a play from [here](http://shakespeare.mit.edu/). Any of the plays should work in this project so long as it is from the link provided, this project uses Romeo and Juliet. (If you are following the scenario then you would aggregate all of them into one doc.) This Project is using this data because they all have the same format and we are parsing the html. 


### Step 2:

- From here we will be working with the understating that you have the required programs installed, if you don't have them install before proceeding.
- Open the spark shell 
  - windows users run the C:\.spark-2.4.0-bin-hadoop2.7\bin\spark-shell.cmd from the command line or git bash 
  - MAC and linux users run ~/.spark-2.4.0-bin-hadoop2.7/bin/spark-shell
  ![](https://github.com/zacharyhaider/Modual6WS/blob/master/spark.png)
  
### Step 3:

From the spark shell create a variable for our regex string.

		val regex = "^((?!<b>(.*?)</b>).)*"  

The string here will select every node in the html other than the bold character names. we will use this in the next step to split the doc for the information we need. 
![](https://github.com/zacharyhaider/Modual6WS/blob/master/regex.png)

### Step 4:
Now we need to import our file into spark scala. Be sure to include the path and full name of the file we downloaded in step one. The quotes are required.

	val inputFile = sc.textFile("{name of your file with path}")

![](https://github.com/zacharyhaider/Modual6WS/blob/master/inputfile.png)

### Step 5:
From here things get complicated. This is all one command.

	val topWordCount = inputFile. // creates our variable that will stor our results  
	flatMap(str=>str.split(regex)).  // uses our regex to split our doc on the information we need  
	filter(!_.isEmpty). //removes all results that are null or just contain spaces 
 	map(word=>(word,1)). // maps each string to the number one and stores it in memory 
	reduceByKey(_+_). // reduces our data and adds all the "1"s together 
	map{case (word, count) => (count, word)}. flips the data so our words become the key 
	sortByKey(false) //sorts our data from least to most

![](https://github.com/zacharyhaider/Modual6WS/blob/master/reduce%20pre.png)

### Step 6:
Now we just need to get our results we can simply return them to the scala command line. 
	
	topWordCount.take(10).foreach(x=>println(x))
![](https://github.com/zacharyhaider/Modual6WS/blob/master/results.png)

## Sources 
https://jaceklaskowski.gitbooks.io/mastering-apache-spark/spark-rdd.html

http://shakespeare.mit.edu/
