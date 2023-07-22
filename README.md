# This is from EAI Assignment @ IUB MSCS

# Ice Tracking

Given galcier Images to find these two boundaries Air-Ice and Ice-Rock. The assumptions observed are stated below:

## Assumptions:
    - Air-Ice is always above Ice-Rock Boundary, seperated by a margin of 10pixels.
    - Both lines span entire image, consisting of each.
    - Each Boundary is smooth, one column transition is similar to the other in next column.
    - A boundary consists of a darker pixel, indicating the stronger edge.
A Image is converted into image_array of size 175rows x 225columns.
## Simple:

**Air_Ice:** For detecting the Air-Ice Boundary, i have considered emission probability as Edge_Strength, giving the edge strength value, a high value indicating a Boundary point. Fetching a Maximum edge value for each column, giving a edge present in the particular row. Adding a condition that previous column row will not be greater than current column row. Iterating this over 225 columns gives us the Air-Ice Boundary.

**Ice_Rock:** For detecting the Ice-Rock Boundary, giving a condition starting from the Air-Ice column row, which gives a condition that it always searches for the larger value after the Air-Ice. With same condition of Air-Ice.

![Image for Simple 30](https://github.com/Harshav0423/Horizon-Detection/blob/master/Images/Simple_air_ice_output.png)

## HMM:

**Emission probability:** Considered Edge_Strength as my Emission probability, calculating for every column.

**Transition probability:** Here the transition probability is switching from a previous column row to current column row. With some ditributions encouraging smoothness.

**Initial probability:** Building a initial list, for the start of Viterbi algorithm contains a its path and initial value 0.

**Viterbi Working:** The Initial state (0) is build based on the row. The Viterbi algorithm runs for every column(225), iterating over the rows(175). It starts from the 2nd column(index=1), it gets the maximum probability for each row, which compares with every previous column row, and stores the probability value and then the path of it. Everytime it considers the maximum value of the particular row and creating a list for the next column.
This cycle is continued until the end of the column, which takes the overall path of a row giving maximum value.

 - a = math.log(1/(1 + abs(j-k[0][-1]))+1e-10) + math.log(edge_strength[j][i]/max_edge_strength[i]+1e-10) +k[1]

 - The above value is calculated for every row in a column for the all previous rows.
 - The first log function calculating the previous row value with current row value, along with its added weight.
 - The second calculates, current column,row edge strength with maximum edge value of the column, total added with previous probability value.
 - If it finds a max value of previous row, iterating over the all rows.

**Air_Ice:** The above algorithm is used in detecting the Air-Ice.

**Ice_Rock:** Similar to the implementation of Air-Ice, it also uses the Air-Ice boundary for correct detection of Ice-Rock boundary, starting the row from, row of Air-Ice boundary + 10 (assumption). This gives us the boundary of Ice-Rock with some smoothening function.

![Image for HMM 30](https://github.com/Harshav0423/Horizon-Detection/blob/master/Images/HMM_ice_rock_output.png)

## Human FeedBack:

Implemented the Viterbi Algorithm, by changing the boundary detection smoothness. 


**Smoothness:** It checks with additional condition, if the current row of current column with row of previous column:
 
 -Calculating the total probability of a row to be choosen
  a = math.log(1/(1 + abs(j-k[0][-1]))+1e-10) + math.log(edge_strength[j][i]/max_edge_strength[i]+1e-10) +k[1]

It multiplies the value with the transition probability, restricting the algorithm not to pick the high transitioned row.

**Air_Ice:** It takes the Human feedback point to rectify its boundary, whenever it sees a point given by Human, it picks the point and assigns them to current col row. This gives the added advantage to take the path along with Human Input. Which has the same implementation of HMM.

**Ice_Rock:** Similar to the implementation of Air-Ice, it also uses the smoothness condition to restrict the next choosing row.

![Image for Human Feedback 30](https://github.com/Harshav0423/Horizon-Detection/blob/master/Images/FB_ice_rock_output_30.png)
![Image for Human Feedback 23](https://github.com/Harshav0423/Horizon-Detection/blob/master/Images/ice_rock_output_23.png)
![Image for Human Feedback 31](https://github.com/Harshav0423/Horizon-Detection/blob/master/Images/ice_rock_output_31.png)
