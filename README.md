# Assignment9.3

# Load the into pig 
load_data = LOAD '/home/acadgild/Pig/Nine/HAsignNineThree/Pokemon.csv' USING PigStorage(',') ;

# Task1:select the Pokemon having defence >55 (defence55.jpeg)
selected_list = FILTER load_data BY $7>55;

# group data
group_selected_list = Group selected_list All;

# Task2:count the gouped item ( count.jpeg)
count_selected_list = foreach group_selected_list GENERATE COUNT(selected_list);

# Task3:create random filed in data for player 1 and 2 (random.jpeg)
random_include1 = foreach selected_list GENERATE RANDOM(),$1,$2,$3,$4,$5,$6,$7,$8,$9,$10;
random_include2 = foreach selected_list GENERATE RANDOM(),$1,$2,$3,$4,$5,$6,$7,$8,$9,$10;

# Task 4:arrange the decreasing order for players (desendingOrder)
random1_desending = ORDER random_include1 BY $0 DESC;
random2_desending = ORDER random_include2 BY $0 DESC;


# limit the data to 5 of random 2 player 
limit_data_random2_desending = LIMIT random2_desending 5;
limit_data_random1_desending = LIMIT random1_desending 5;


# generate the data to be shown name and Hp 
filter_only_name1 = foreach limit_data_random1_desending Generate ($1, $6);
filter_only_name2 = foreach limit_data_random2_desending Generate ($1, $6);

# Task 5:store the result into files 
store limit_data_random1_desending INTO '/home/acadgild/Pig/Nine/HAsignNineThree/output/player1.txt';
store limit_data_random2_desending INTO '/home/acadgild/Pig/Nine/HAsignNineThree/output/player2.txt';

