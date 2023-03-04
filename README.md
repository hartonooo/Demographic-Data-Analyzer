# Demographic-Data-Analyzer
certification python project from <a href="https://www.freecodecamp.org/learn/data-analysis-with-python/data-analysis-with-python-projects/demographic-data-analyzer" target="_blank" rel="noopener noreferrer">freecodecamp</a>


using Python (pandas) to answer the following questions:
<details><summary>import pandas, import data</summary>
<pre>
import pandas as pd
df = pd.read_csv("adult.data.csv")
df.head()
</pre>
</details>

<details>
<summary>dataset</summary>
<img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/0.%20data.jpg">
</details>
</br>

1. How many people of each race are represented in this dataset? This should be a Pandas series with race names as the index labels. (race column)

    <details><summary>people</summary>
    <pre>
    df["race"].value_counts()
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/1.%20people.jpg">
    </details>

2. What is the average age of men?

    <details><summary>average age of men</summary>
    <pre>
    round(df[df["sex"]=="Male"]["age"].mean(), 1)
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/2.%20avg%20age%20of%20men.jpg">
    </details>

3. What is the percentage of people who have a Bachelor's degree?

    <details><summary>pct of Bachelor's degree</summary>
    <pre>
    pct_Bachelors = round((df[df["education"]=="Bachelors"]["education"].count() / df["education"].count()) * 
                      100, 1)
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/3.%20pct%20of%20bachelor.jpg">
    </details>

4. What percentage of people with advanced education (Bachelors, Masters, or Doctorate) make more than 50K?

    <details><summary>pct of advanced education with earning > 50K</summary>
    <pre>
    adv_edu = ["Bachelors", "Masters", "Doctorate"]
    </br>
    higher_education = df[df["education"].isin(adv_edu)]["education"].count()
    lower_education = df[~df["education"].isin(adv_edu)]["education"].count()
    </br>    
    higher_education_rich = round((df[((df["education"] == "Bachelors") | 
        (df["education"] == "Masters") | 
        (df["education"] == "Doctorate")) & 
        (df["salary"] == ">50K")]["salary"].value_counts() / 
        df[(df["education"] == "Bachelors") | 
        (df["education"] == "Masters") |
        (df["education"] == "Doctorate")]["salary"].count()) * 100, 1)[0]
    </br>
    lower_education_rich = round(df[(~df["education"].isin(adv_edu)) & (df["salary"] == ">50K")]["salary"].count() / df[(~df["education"].isin(adv_edu))]["salary"].count() * 100, 1)
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/4.%20advanced%20education%20rich.jpg">
    </details>


5. What percentage of people with lower education make more than 50K?
    
    <details><summary>pct of lower education with salary >50K</summary>
    <pre>
    lower_education_rich = round(df[(~df["education"].isin(adv_edu)) & (df["salary"] == ">50K")]["salary"].count() / df[(~df["education"].isin(adv_edu))]["salary"].count() * 100, 1)
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/5.%20lower%20education%20rich.jpg">
    </details>

6. What is the minimum number of hours a person works per week?
    
    <details><summary>hours of work per week</summary>
    <pre>
    df["hours-per-week"].min()
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/6.%20hours%20of%20work%20per%20week.jpg">
    </details>


7. What percentage of the people who work the minimum number of hours per week have a salary of more than 50K?
    
    <details><summary>minimum hours of work per week & salary >50K</summary>
    <pre>
    num_min_workers = df[(df["hours-per-week"] == df["hours-per-week"].min()) & (df["salary"] == ">50K")]["salary"].count()
    </br>
    rich_percentage = (df[(df["hours-per-week"] == df["hours-per-week"].min()) & (df["salary"] == ">50K")]["salary"].count()/
        df[df["hours-per-week"] == df["hours-per-week"].min()]["salary"].count()) * 100
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/7.%20minimum%20hours%20of%20work%20per%20week%20%26%20salary%20more%20than%2050K.jpg">
    </details>
    
8. What country has the highest percentage of people that earn >50K and what is that percentage?

    <details><summary>country and pct of people with salary >50K</summary>
    <pre>
    country = []
    for i in df[df["salary"] == ">50K"]["native-country"].unique():
        country.append(i)
    </br>
    container = {}
    </br>
    for i in country:
        container[i] = [df[(df["salary"] == ">50K") & (df["native-country"] == i)]["salary"].count(), 
                        df[df["native-country"] == i]["salary"].count(), df[(df["salary"] == ">50K") & (df["native-country"] == i)]["salary"].count()/df[df["native-country"] == i]["salary"].count() * 100]
    </br>
    highest_earning_country = sorted(container.items(), key = lambda item:item[1][2], reverse = True)[0][0]
    highest_earning_country_percentage = round(sorted(container.items(), key = lambda item:item[1][2], reverse = True)[0][1][2], 1)
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/8.%20country%20and%20pct%20of%20people%20with%20salary%20more%20than%2050K.jpg">
    </details>

9. Identify the most popular occupation for those who earn >50K in India.
    <details><summary>most popular occupation for people with salary >50K</summary>
    <pre>
    df[(df["native-country"] == "India") & (df["salary"] == ">50K")]["occupation"].value_counts().index[0]
    </pre>
    </details>

    <details>
    <summary>result</summary>
    <img src="https://github.com/mas-tono/Demographic-Data-Analyzer/blob/main/image/9.%20most%20popular%20occupatioin%20for%20people%20with%20salary%20more%20than%2050K.jpg">
    </details>


10. building and testing codes in jupyter notebook

11. put them together in demographic_data_analyzer.py file

12. call via main.py
