# ETL_Project
Used an ETL process to tabulate wine review data, determine the most & least popular wine varieties, 
and load the data into PostgreSQL.


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

-Performed groupby with mean aggregation to look at average point and price for each title.<br />

-Performed sort_values to determine the Top 5 & Bottom 5 popular titles.<br />


<b>Wines.xlsx & Combining</b><br />
<br />
-Took the ‘Winemag-130k-v2.csv’ data & cleanup code with just a few changes.<br />

   -Specified columns to keep instead of columns to drop.<br />

   -Kept the ‘id’ column.<br />

   -Made a copy of the first dataframe.<br />

   -Dropped all NaN values rather than just those from certain columns.<br />

   -Performed a row count.<br />

   -Used each step to store the dataframe in a new variable.<br />

-Used the same steps to cleanup ‘Wines.xlsx’.<br />

-Appended the ‘Wines.xlsx’ dataframe onto the ‘Winemag-130k-v2.csv’ dataframe to make ‘wine_df_final’.
   -It was originally proposed to use a ‘join’ statement, but there were issues with finding a unique primary 
	key to join the data on (which is admittedly odd, since the ‘title’ contains a composite of ‘winery’, ‘vintage’, 
	and ‘designation’).
   -Since the data came from separate sites, it was assumed that there wouldn’t be duplicates.

-Used the ‘groupby’ function in conjunction with the ‘mean’ function to return a dataframe with the mean 
‘points’ and ‘price’ values by each ‘title’ as ‘wine_df_grouped’.<br />

-Ran the ‘sort_values’ function on the resultant dataframe on ‘points’ to show the top 5 & bottom 5 popular 
wine titles.<br />


<b>Loading Steps</b><br />

-Created a database called ‘wines_df’ in PgAdmin4 with two tables- 
‘wines_table’ (for ‘wine_df_final’) and ‘wines_titles_table’ (for ‘wine_df_sorted’), so that two dataframes were to be loaded.<br />

-Created a ‘create table’ statement for each table with columns to match the corresponding Pandas dataframes.<br />

-Created an engine, with the table names being confirmed.<br />

-Exported the dataframes to their corresponding tables in PgAdmin with a ‘to_sql’ statement.<br />

