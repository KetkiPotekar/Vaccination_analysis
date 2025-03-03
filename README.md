# Vaccination_analysis
Analyze global vaccination data to understand trends in vaccination coverage, disease incidence, and effectiveness. Data will be cleaned, and stored in a SQL database. Power BI will be used to connect to the SQL database and create interactive dashboards that provide insights on vaccination strategies and their impact on disease control.

# step 1 data cleaning
This was done using python and python libraries namely pandas, numpy and openpyxl.
There are 5 datasets provided, each dataset detailing vaccination, reported cases, coverage, schedule and introduction about vaccination information

# Dataset 1 (about coverage data) 
    pip install pandas numpy openpyxl

    import pandas as pd
    import numpy as np

    file_path = r"C:\Users\ratnakar\Documents\IIT Madras\Mini_Project 2\coverage-data.xlsx" # for loading the dataset
    coverage_df = pd.read_excel(file_path, engine = "openpyxl") 
    print(coverage_df.head())

    1) to find null values

    print(coverage_df.isnull().sum()) 
    #to remove null values
    df_cleaned = coverage_df.dropna()

    2) to find duplicates

    duplicates = coverage_df[coverage_df.duplicated()]
    #remove duplicates
    df_cleaned = coverage_df.drop_duplicates()

    3) fill missing values with the mean for each YEAR and ANTIGEN group
    coverage_df['COVERAGE'] = coverage_df.groupby(['YEAR','ANTIGEN'])['COVERAGE'].transform(lambda x: x.fillna(x.mean()))

    #fill missing TARGET_NUMBER and DOSES with median
    coverage_df['TARGET_NUMBER'].fillna(coverage_df['TARGET_NUMBER'].median(),inplace = True)
    coverage_df['DOSES'].fillna(coverage_df['DOSES'].median(),inplace = True)

    #Drop rows if essential fields like country name, or year are missing
    coverage_df.dropna(subset=['NAME','YEAR'],inplace = True)

    #check ,missing values after handling them
    print(coverage_df.isnull().sum())

    4) normalize units
    
    #convert coverage to a percetnage (if it is incorrectrly scaled)
    coverage_df['COVERAGE']=coverage_df['COVERAGE'].apply(lambda x: x if x<=100 else x/10)

    #convert doses and target number to integers
    coverage_df['DOSES'] = coverage_df['DOSES'].astype(int)
    coverage_df['TARGET_NUMBER'] = coverage_df['TARGET_NUMBER'].astype(int)

    print(coverage_df[['COVERAGE','DOSES','TARGET_NUMBER']].head())

    5) save the cleaned data
    
    coverage_df.to_excel("cleaned_coverage_data.xlsx", index = False)

# Dataset 2 (about incident rate)

    # load the datset

    file_path = r"C:\Users\ratnakar\Documents\IIT Madras\Mini_Project 2\incidence-rate-data.xlsx"
    incidence_rate_df = pd.read_excel(file_path,engine="openpyxl")

    #display first few rows
    print(incidence_rate_df.head())

    1) to find null values and remove them
    print(incidence_rate_df.isnull().sum())
    df_cleaned=incidence_rate_df.dropna()

    2) to find duplicates 
    duplicates = incidence_rate_df[incidence_rate_df.duplicated()]
    print(duplicates)

    # to remove duplicates and save the dataset
    df_cleaned=incidence_rate_df.drop_duplicates()
    df_cleaned.to_excel("cleaned_incidence_rate_data.xlsx", index = False) # SAVE THE DATASET

# Dataset 3 (about reported cases)

    # load the dataset
    import pandas as pd
    import numpy as np

    file_path = r"C:\Users\ratnakar\Documents\IIT Madras\Mini_Project 2\reported-cases-data.xlsx"
    reported_cases_df = pd.read_excel(file_path, engine = "openpyxl")

    # display first few rows
    print(reported_cases_df.head())

    1) to find and remove null values

    print(reported_cases_df.isnull().sum())
    reported_cases_df.dropna(subset=['CASES'], inplace=True)

    2) to find duplicates

    duplicates = reported_cases_df[reported_cases_df.duplicated()]
    print(duplicates)

    # to remove duplicates and save the dataset
    reported_cases_df.drop_duplicates(inplace=True)
    reported_cases_df.to_excel("cleaned_reported_cases_data.xlsx",index = False)

# Dataset 4 (about vaccine introduction)
    # to load the dataset

    file_path = r"C:\Users\ratnakar\Documents\IIT Madras\Mini_Project 2\vaccine-introduction-data.xlsx"
    vaccine_intro_df = pd.read_excel(file_path, engine = "openpyxl")

    # display first few rows
    print(vaccine_intro_df.head())

    1) to find and remove null values

    print(vaccine_intro_df.isnull().sum())
    vaccine_intro_df.dropna(subset=['INTRO'], inplace=True)

    2) to find duplicates

    duplicates = vaccine_intro_df[vaccine_intro_df.duplicated()]
    print(duplicates)

    # to remove duplicates and save the dataset

    vaccine_intro_df.drop_duplicates(inplace=True)
    vaccine_intro_df.to_excel("cleaned_vaccine_introduction_data.xlsx",index = False)
    
# Dataset 5 (about vaccine schedule)
    # load the dataset

    file_path = r"C:\Users\ratnakar\Documents\IIT Madras\Mini_Project 2\vaccine-schedule-data.xlsx"
    vaccine_schedule_df = pd.read_excel(file_path, engine = "openpyxl")

    # display first few rows
    print(vaccine_schedule_df.head())

    1) to find and remove null values

    print(vaccine_schedule_df.isnull().sum())
    vaccine_schedule_df.dropna(subset=['TARGETPOP','AGEADMINISTERED'], inplace=True)

    2) to find duplicates

    duplicates = vaccine_schedule_df[vaccine_schedule_df.duplicated()]
    print(duplicates)

    # to remove duplicates and save the dataset

    vaccine_schedule_df.drop_duplicates(inplace=True)
    vaccine_schedule_df.to_excel("cleaned_vaccine_schedule_data.xlsx",index = False)

# Questions answered and Inferences made from the data visualization

1) How do vaccination rates correlate with a decrease in disease incidence?
   The vaccination rate shows a strong inverse correlation with disease incidence, there is a consistent decrease in disease incidence as the vaccination rates increased. This could also factor from something called as herd immunity, When a critical portion of the population is vaccinated, the disease struggles to spread.
Even unvaccinated individuals (e.g., those with medical exemptions) benefit because exposure risk is lower.

2) Which regions have high disease incidence despite high vaccination rates?
   The African subcontinent showed high disease incidence despite high vaccination rates.

3) What is the trend in disease cases before and after vaccination campaigns?
   The trend in disease cases has been increasing and decreasing seasonally before vaccination campaigns began, but after the vaccination campaigns have covered a significant area, the disease trends have decreased when compared to before.

4) Which diseases have shown the most significant reduction in cases due to vaccination?
   Diseases such as polio, rubella and yellow fever have shown most significant reduction after vaccination, this is because these vaccines have a high efficiency against their causative microorganisms. Polio showed almost complete iradication wherever complete vaccination coverage has occured.

5) What percentage of the target population has been covered by each vaccine?
   Upto 70% of all target population has been covered by each vaccine.

6) Are certain diseases more prevalent in specific geographic areas?
   Certain diseases such as measules, mumps, typhoid and diptheria are more prevelant in african and asian subcontinents.

7) A government health agency wants to identify regions with low vaccination coverage to allocate resources effectively.
   Any govt health agency that wants to focus regions with low vaccination coverage should focus on regions such as african and asian subcontinent.

8) A public health organization wants to evaluate the effectiveness of a measles vaccination campaign launched five years ago.
   Measules vaccine average incidence rate 5 years ago, i.e in 2019 was 79.29 and average coverage was 70.50, this incidence rate decreased to 76.13 and the average coverage increased to 70.31.

9) Researchers want to explore the incidence rates of polio in populations with no vaccination coverage.
    The countries with no vaccionation coverage of polio are almost non-existent, although there are countries such as Tuwalo, Kiribati, Kenya, Zambya, Zimbabwe where the incident rates of polio are the highest and there is a need of vaccination coverage in these countries.
