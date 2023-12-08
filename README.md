# ETL_Project
Used an ETL process to tabulate wine review data, determine the most & least popular wine varieties, 
and load the data into PostgreSQL.
<br />
<br />
<b>Extraction Steps</b><br />
<br />
-Found data for different wine ratings on Kaggle and Dataworld websites. The orginal files came 
in various formats including CSV, JSON and XLXS.<br />
	-The Kaggle data was from winemag.com (Wine Magazine’s web site). Loaded the CSV file into GitHub.<br />
	-The Dataworld data was originally available in CSV, XLXS and JSON format, but some of the symbols were 
	not UTF-8 characters. Addressed this by reading the XLXS file version.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/02cff73a-472e-4b7f-9c0b-66ee773cbbeb)<br />
<br />
-Added a column to both extracted Pandas Dataframes indicating the source of the data: WM (Wine Magazine) or DW (Dataworld).<br /> 
	-Did this to allow duplicates to be removed and to average the rating and price of any wine in both WM and DW 
	and change the source to indicate it was both.<br />
<br />
	-The Winemag file did not have vintage as a column. The title of a wine is the winery, the vintage and 
	the designation.  Extracted the vintage from the column of string variables under title. Stored the results as integers in a vintage column.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/4426ff15-bf40-4be1-b8cd-6d7b8d119b78)<br />
<br />
	-The Dataworld file had the Vintage column as a mixture of datetime variables and strings. A subroutine 
	converted them into integers for the vintage year and replaced the Vintage column.
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/bcb42e17-1a48-4ef7-84e1-b1df50f6b780)<br />
<br />
<br />
<b>Transformation Steps</b><br />
<br />
Winemag-130k-v2.csv
<br />
-Called the ‘Winemag-130k-v2.csv’ dataframe.<br />
-Changed the initially unnamed first column to ‘id’.<br />
-Deleted the 'id', 'description', 'province', 'region_1', 'region_2', 'taster_name', 'taster_twitter_handle', 
and 'variety' columns.<br />
	-This was so that only the ‘title’, ‘vintage’, ‘country’, ‘winery’, ‘designation’, ‘points’, ‘price’, and 
	‘source’ columns remained.<br />
-Wrote several lines of code to keep only the rows that had values in specified columns.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/7100c129-dd6f-49ac-a239-9537cffbf75b)<br />
<br />
<br />
Wines.xlsx & Combining<br />
-Took the steps with the ‘Winemag-130k-v2.csv’ data & cleanup code, making just a few changes.<br />
   -Specified columns to keep instead of columns to drop.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/a52862c1-9a31-4ce1-a58f-9916c1351cf2)<br />
<br />
   -Kept the ‘id’ column.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/772cf3ec-9bf9-4c83-8880-d20ad18f1114)<br />
<br />
   -Made a copy of the first dataframe.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/ad7841a7-da43-4870-bee0-18824e7bc8d8)<br />
<br />
   -Dropped all NaN values rather than just those from certain columns.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/0ea9e938-746c-47fb-a29b-c51b6db4c3d2)<br />
<br />
-Appended the ‘Wines.xlsx’ dataframe onto the ‘Winemag-130k-v2.csv’ dataframe to make ‘wine_df_final’.
   -It was originally proposed to use a ‘join’ statement, but there were issues with finding a unique primary 
	key to join the data on (which is admittedly odd, since the ‘title’ contains a composite of ‘winery’, ‘vintage’, 
	and ‘designation’).<br />
   -Since the data came from separate sites, it was assumed that there wouldn’t be duplicates.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/5f53d43f-0832-4727-b556-b1eeca44699d)<br />
<br />
-Used the ‘groupby’ function in conjunction with the ‘mean’ function to return a dataframe with the mean 
‘points’ and ‘price’ values by each ‘title’ as ‘wine_df_grouped’.<br />
-Ran the ‘sort_values’ function on the resultant dataframe on ‘points’ to show the top 5 & bottom 5 popular 
wine titles.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/760ca12b-49b6-4704-94ae-8c9a59a2bd3f)<br />
<br />
<br />
<b>Loading Steps</b><br />
<br />
-Created a database called ‘wines_df’ in PgAdmin4 with two tables- 
‘wines_table’ (for ‘wine_df_final’) and ‘wines_titles_table’ (for ‘wine_df_sorted’), so that two dataframes were to be loaded.<br />

-Created a ‘create table’ statement for each table with columns to match the corresponding Pandas dataframes.<br />

-Created an engine, with the table names being confirmed.<br />
![image](https://github.com/KotR9001/Wine_Ratings_ETL/assets/57807780/346f195a-bc9c-48b0-be23-a318b1a16513)<br />


-Exported the dataframes to their corresponding tables in PgAdmin with a ‘to_sql’ statement.<br />

