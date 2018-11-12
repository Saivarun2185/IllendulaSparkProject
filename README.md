# SAIVARUN ILLENDULA

## About Me
I am Saivarun Illendula, a graduate student pursuing Masters in Applied Computer Science.
## Objective

The objective of this project is to count the frequency of the highest number of repeated words from the data collected from Wikipedia about PS4.

## Data Source

I have taken the data from wikipedia regarding PS4. I have copied the first few lines and pasted them into the Spark_PS4 text file.

Source: [PS4 Wikipedia](https://en.wikipediaorg/wiki/PlayStation)



## Commands

1. Reading the Data from the Text file:
    ```Scala
    var textdata = sc.textFile("spark_PS4.txt")
    ```

2. For splitting the data based on the white space in the input file and storing the resultant text into a seperate dataset:
    ```Scala
    var splitdata = textdata.flatMap(_.split(" "))
    ```

3.  For splitting the data based on the commas in the input file and storing the resultant text into a seperate dataset:
    ```Scala
    var splitwithcomma = splitdata.flatMap(_.split(","))
    ```

4. For filtering the text which contains standalone numbers or letters.
    ```Scala
    var splitwithcommaregex = splitwithcomma.filter(_.forall(_.isLetterOrDigit))
    ```

5. For filtering the text which contains alphanumeric text.
    ```Scala
    var nonumbers = splitwithcommaregex.filter(_.matches("^[a-zA-Z]*$"))
    ```
6. For filtering the text that have less than 1 character.
    ```Scala
    var splitwithcommaregex = splitwithcomma.filter(_.length >= 1)
    ```
6. Mapping the entire text with the number 1 
    ```Scala
    var mapper = nonumbers.map((_,1))
    ```

7. Using continous addition inorder to reduce the output from the mapper (key,value)  
    ```Scala
    var reducer = mapper.reduceByKey( _ + _ )
    ```

8. For reversing the order of the output key value pairs into value key pairs.
    ```Scala
    var reversekv = reducer.map { case (key,value) => (value,key)}
    ```

9. Sorting the output obtained from the above step into descending order to count the number of occurences of frequently repeated words.
    ```Scala
    var sorteddata = reversekv.sortByKey(false)
    ```

10. Filtering only the occurances of the top 10 highly repeated words.
    ```Scala
    sorteddata.take(10).foreach(println)
    ```
## Results:

### Chart:
![Wordcountchart](https://github.com/Saivarun2185/IllendulaSparkProject/blob/master/Images/WordCount.png)


### Data:

| Word        | Count |
|-------------|-------|
| the         | 20    |
| and         | 18    |
| to          | 11    |
| PlayStation | 10    |
| of          | 9     |
| with        | 7     |
| The         | 6     |
| console     | 6     |
| its         | 5     |
| also        | 5     |


### Link to the Published Page:
Link: 
https://saivarun2185.github.io/IllendulaSparkProject/