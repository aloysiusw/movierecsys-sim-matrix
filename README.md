# Movie Recommender System with Similarity Matrices

The prompt for this project was to create a recommender system using two different algorithms. We decided to write the implementation from scratch based on what we learned in class.

The notebook/colab file has been segmented into parts for easier parsing and each part was written so that it can use its own .csv such that when we needed to debug and test, we did not need to rerun all the processing all over again.

# The First Approach - Jaccard Similarity

![image](https://user-images.githubusercontent.com/48269287/212354179-80ebd0fe-210c-484d-aadc-90dbc34d4def.png)


The first algorithm we implemented is a solution based on computing Jaccard similarity. For this part, we had written a script to separate and convert the genre values in the dataset from one mixed entry into separate columns and another script to fill those columns with 1s and 0s based on whether the genre is valid for that movie or not. The dataframe containing the encoded genres were then transposed and saved to another csv so we can reference that whenever we needed after. Since we had done a sorting and reset the index, we were confident that this new dataset matched the original one, but we also did checks to ensure that that was the case.

	The recommender algorithm then takes in a user input or data and checks whether the movie we had queried is in the dataframe or not. If it is, then we iterated through the dataframe and compared the genres of the movie we had searched with the genres of all the 9000+ movies in the dataset. We then kept the ones that are similar up to a certain threshold based on the argument we had passed, and also kept the max rating. If a movie had at that point the same similarity rating, we compared the popularity and chose the more popular one to be recommended.
  
![image](https://user-images.githubusercontent.com/48269287/212354226-9896fb72-62e6-493e-9e4f-4279bdd8fa60.png)


  
	The output is then saved into a new result dataframe, and outputted to the sure based on max popularity.
  
  # The Second Algorithm - TF-IDF and Cosine Similarity
  
  ![image](https://user-images.githubusercontent.com/48269287/212354337-1fb0b20f-3f7d-45a2-8945-06bb71366a10.png)

  
  The second algorithm uses TF-IDF and Cosine Similarity instead of jaccard similarity. For this approach, we tested it with a sample user dataframe, as the similarity is determined instead by what the user have watched in the past. First we combined the title of the movie and the plot summary of the movie into a new column, and used a python package to vectorize it. It was then fit into a TF-IDF matrix and applied the cosine similarity function to the matrix.
  
We then created a function to take in the user_id and grabbed the movies that the user 
have watched from the sample dataframe. The result is then passed into the method and we calculated the cosine similarity score of that movie and compared it with the movies in the dataset. For each entry that we found to have a high similarity score, we then outputted into a result dataframe showing what movies we recommend based on what movie we had queried.


# Some preprocessing magic

As part of preparing the data for parsing, we had done several things to ensure the data is readable by the code.

![image](https://user-images.githubusercontent.com/48269287/212355251-eff2e517-b7ea-470a-9053-4099bed37487.png)

We imported the .csv to colab and read it as a dataframe for easier data manipulation. We then sorted it by title to make sure that everything is normal and then dropped duplicates. Our key for checking the duplicates is the "Overview" column instead of "Title". This is because there can be several movies with the same title, but most movies have a different summary written about them. We then reset the index, and write that as a new .csv

The data set that we used had all the genres lobbed into block. To make it useable and readable for our purposes, we had to split it into its own categories and then fed it back to the full dataframe and ran a script to correctly note what genres each movie ticked using one hot encoding.

![image](https://user-images.githubusercontent.com/48269287/212355716-32452227-9d57-4da9-b172-a1215263ee08.png)

The first step was to split the strings in the genre columns into its own dataframe, automatically adding a new column for each genre in the movie. Inevitably, this led to duplicates being created because the genres are not all going to be in the same order, and they will be placed as a stack (first come, first serve). To solve this, we added all the genres into a list and then converted that list into a set. We took advantage of the set's property of not allowing duplicates to naturally remove multiple entries

![image](https://user-images.githubusercontent.com/48269287/212357138-8bc01756-2a27-4ef4-b3b7-07b485eab0a3.png)

![image](https://user-images.githubusercontent.com/48269287/212357398-0a54d007-f0ba-41db-8ccd-3a0e72524800.png)

We then fed it back to the dataframe, and filled it with zeroes so we do not encounter any NaN issues.

![image](https://user-images.githubusercontent.com/48269287/212357460-ee4b0a17-8a70-4e19-8561-77780077ee6f.png)


Afterwards we wrote a simple loop script to iterate over the dataframe, then check for each column if there is a matching one in the block, and flipped the bit to 1 if it did.

![image](https://user-images.githubusercontent.com/48269287/212357653-fafc186d-3115-40d4-81b7-25d27c426fa0.png)


The result above showcases the script working and marking the movie with the right genre.

