## My Pandas Personal Notebook :-

## When to find of many values in a column:-
df_int['Ensemble_MAPE']=df_int[['MAPE_Xgb','MAPE_Ridge','MAPE_Lasso','MAPE_RF']].min(axis=1)

## Find the column name having least values out of a series of column:-
df_int['Best_Performing_model']=df_int[['MAPE_Xgb','MAPE_Ridge','MAPE_Lasso','MAPE_RF']].idxmin(axis=1)

## Merging by selecting a set of columns from 1 dataframe with other dataframe:-
pd.merge(df_ensemble.loc[:,['LANES','MAPE_Xgb','MAPE_Ridge','MAPE_Lasso','MAPE_RF']],df_mix,on=['LANES'])


## Applying apply function on multiple columns in a dataframe
dict1={'MAPE_RF':'Infy_RF_PREDICTIONS','MAPE_Lasso':'Infy_lasso_PREDICTIONS','MAPE_Xgb':'Infy_Xgb_PREDICTIONS','MAPE_Ridge':'Infy_Ridge_PREDICTIONS'}

df_mix['BEST_PREDICTIONS']=df_mix.apply(lambda x:x[dict1[x['Best_Performing_model']]],axis=1)

## Reversing  a dictionary
inv_map = {v: k for k, v in dict1.items()}


## SQL using Python
import sqlite3 
connection = sqlite3.connect("myTable.db")

#### cursor  
crsr = connection.cursor() 
  
#### SQL command to create a table in the database 
sql_command = """CREATE TABLE MArket2 (  
SELLER_ID INTEGER PRIMARY KEY,  
COUNTRY VARCHAR(20),  
JOINING_DATE DATE);"""
  
  
#### execute the statement 
crsr.execute(sql_command) 
  
  
#### SQL command to insert the data in the table 
sql_command = """INSERT INTO MArket2 VALUES (23, "India", "2014-03-28");"""
crsr.execute(sql_command) 
  
#### another SQL command to insert the data in the table 
sql_command = """INSERT INTO MArket2 VALUES (24, "USA", "2014-03-29");"""
crsr.execute(sql_command) 

#### If we skip this, nothing will be saved in the database. 
connection.commit() 

crsr.execute("SELECT * FROM Employee, MArket2;")


ans= crsr.fetchall() 
for i in ans: 
    print(i) 


## NbExtension
!pip install jupyter_contrib_nbextensions
!jupyter contrib nbextension install --user
!pip install jupyter_nbextensions_configurator
!jupyter nbextensions_configurator enable --user



## Reading and writing multiple excel sheets using pandas
import pandas as pd

xls = pd.ExcelFile('Data.xlsx')
xls.sheet_names

sheet_to_df_map = {}
for sheet_name in xls.sheet_names:
    print(sheet_name)
    sheet_to_df_map[sheet_name] = xls.parse(sheet_name)

writer = pd.ExcelWriter('pandas_multiple.xlsx', engine='xlsxwriter')
for Country, data in sheet_to_df_map.items():
    df1=pd.DataFrame()
    print(Country,type(data))
    df1=data
    df1['one_time_cost/recurring_cost']=df1['one_time_cost']/df1['recurring_cost']
    df2=df1.pivot(index='name', columns='Year-Quarter', values=['ease_of_use','ease_of_setup','ease_of_doing_business_with','pricing','one_time_cost/recurring_cost'])
    df2.to_excel(writer, sheet_name=Country)
writer.save()



