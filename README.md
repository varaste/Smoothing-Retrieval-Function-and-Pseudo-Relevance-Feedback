# Smoothing-Retrieval-Function-and-Pseudo-Relevance-Feedback

Galago is a desktop presence framework designed to transmit presence information between programs. It takes information on who is online and their away/idle states from an instant messenger or similar programs and lets other programs (such as Evolution) use it.

We intend to study different methods of smoothing retrieval functions and their parameters, and we will also expand the user query using the Pseudo Relevance Feedback method.

## Review of smoothing methods

By default, the Galago tool performs the recovery by the Query-Likelihood method. In this project, the goal is to familiarize with smoothing methods and compare the impact of each on the quality of ranking.

One of the problems raised in information retrieval is the existence of zero probabilities, which makes calculations difficult in practice. Smoothing methods were proposed to solve this problem to estimate the probability of occurrence of unseen query words in documents.

The methods we are going to examine are:

- JM method

- Dirichlet prior method

- Additive Smoothing method

We check the different values of parameters by using the appropriate diagram for each of the requested methods and determine the optimal value.

To avoid wasting computing resources, we change the parameters first with big steps and then with small steps.

## Two-stage smoothing


Each of the smoothing methods had advantages; here, the purpose of investigating the two-stage smoothing is obtained by combining two smoothing methods, Dirichlet and JM.

We will investigate this smoothing method and compare and analyze the obtained results with the previous state.


Dataset used in this project consists of three parts:

### Corpus

Collections of news articles are in TREC format. Each document contains several fields:

- **OCNO**: ID of each document

- **Head**: The title of the document

- **Text**: The text of the document

### Queries

This file contains questionnaires.


### Relevance Judgment
This file contains relevant judgments. In the final stage, to evaluate retrieval functions, the obtained results are compared with these judgments.






## Create an index 

In any language, words will have different appearances according to the role they play in sentences. But all of them are made from the same root. Therefore, in many methods, we must first find the root of the words. There are different algorithms for stemming; Porter's algorithm is one of the famous algorithms in the English language. According to a series of regular rules (for example, removing the letter s at the end of plural words), this algorithm can obtain the roots of words with good accuracy.

After installing and setting up Galago, we used the Porter Stemmer method to find the roots of words with the help of Galago commands.

```ruby
"stemmer" : ["porter"]
```
  
Tokenization converts text into its constituent tokens. We do this with the following commands:

```ruby
"tokenizer" : {
    "fields" : ["text","head"],
    "formats" : {
      "text"    : "string",
      "head"    : "string"
    }
  } 
```

![Tokenization](https://github.com/varaste/Document-Ranking-with-Galago/blob/main/assets/Arya%20Varaste-Tokenization.png)



Finally, we extracted the file format, which was initially unformatted with 7zip software from the Windows operating system and reached the .txt file. This type of file is suitable for documents and texts that we need to be able to see different parts of it separately.

![trectext](https://github.com/varaste/Document-Ranking-with-Galago/blob/main/assets/Arya%20Varaste-%20trectext%20.png)

We put the commands related to the settings mentioned above in the indexSettings.json file and run the following command in the terminal:

```
Galago/galago-3.16/core/target/appassembler/bin/galago build  /home/arya/Desktop/CA1-Resources/indexSettings.json
```

After about an hour of executing the Build command, the indexing process was completed successfully.

```
Done Indexing.
  - 0.92 Hours
  - 55.18 Minutes
  - 3310.79 Seconds
Documents Indexed: 163912.
```

## First method: JM with parameter λ
In the JM method, we perform a linear interpolation between MLE and CLM, which is controlled by the Landa parameter. λ ∈ [0, 1]. Therefore, the larger the Landa value is set in this method, the more important the look at the language model set or background. The smaller the Landa value, the more important the current document is. So by mixing the two distributions, we achieve the goal of assigning a non-zero probability to the unseen words in the document we are currently scoring.
Below and in the formula of the JM method, you can see the details of the linear interpolation method:
 
 ![Tokenization](https://github.com/varaste/Smoothing-Retrieval-Function-and-Pseudo-Relevance-Feedback/blob/main/assets/JM.png)
 
Now, with the JM method, we first perform the recovery with the default value of λ on the query set from 51 to 100 with the value of 1000 for the number of Requested and get the values ​​of MAP and P@20. In the following, we change λ by trial and error first with long steps and note the values ​​every time, and if we see an improvement compared to the default state, we try other values ​​with smaller steps to reach the optimal value.

The range of appropriate values ​​for the λ parameter is between zero and one, and its default value is 0.5. In this section, we tested the values ​​in these areas as well as a margin from above and below, first with long steps and then with short steps, and the optimal values ​​for questionnaires 51 to 100 for the λ parameter were 0.475 and 0.485.

The results obtained from these optimal values ​​were more excellent than those obtained from the default value, indicating that the optimization of these parameters successfully got better results.

Although according to the theory, a value higher than one for the parameter λ is meaningless, we tested them and the practical results obtained were in confirmation of the theory.

The figure below shows the diagram of MAP and P@20 resulting from the test of different values ​​for the λ parameter.

 ![Tokenization](https://github.com/varaste/Smoothing-Retrieval-Function-and-Pseudo-Relevance-Feedback/blob/main/assets/JM MAP And P@20.png)
 
 ## Second method: ‌Dirichlet with parameter λ

With the Dirichlet method, we first perform the recovery with the default value μ on the set of queries from 51 to 100 with a value of 1000 for the Requested number and obtain the values of MAP and P@20. In the following, we change µ by trial and error between 0 and 30000, first with long steps and note the values every time, and if we see an improvement compared to the default state, we try relative values with smaller steps to get Let us reach the optimal value.

The default value of the μ parameter is 1500. In this section, we tested the values in these areas and a margin from above and below these areas, first with long steps and then with short steps, and the optimal value for the questionnaires 51 to 100 for the μ parameter was found to be 3450. Of course, in this value, the size of the MAP evaluation criterion was slightly reduced compared to the default state, but the P@20 evaluation criterion improved significantly.

In the figure below, you can see the diagram of MAP and P@20 resulting from the test of different values for the μ parameter:

 ![Tokenization](https://github.com/varaste/Smoothing-Retrieval-Function-and-Pseudo-Relevance-Feedback/blob/main/assets/JM.png)

 ## Third method: ‌Additive Smoothing

In this part, with the Additive Smoothing method, we first perform the retrieval with the default value of theta on the query set from 51 to 100 with the value of 1000 for the number of Requested, and get the values of MAP and P@20. In the following, we change theta between 0 and 10 by trial and error, first with long steps and note the values every time, and if we see an improvement compared to the default state, we try close values with smaller steps to Let us reach the optimal value.

 ![Tokenization](https://github.com/varaste/Smoothing-Retrieval-Function-and-Pseudo-Relevance-Feedback/blob/main/assets/JM.png)









