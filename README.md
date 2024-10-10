# Introduction & Overview

__________________________________________________________________________________
In the midst of a career transition inspired by my newfound passion for data and the stories they hold, I found myself navigating a sea of information, tools, courses, and inevitable doubts. During this journey, I came across a dataset sourced from [Luke Barousse's Python Data Analytics Course](https://github.com/lukebarousse/Python_Data_Analytics_Course), which caught my interest due to its potential to answer key questions I've had since embarking on this path.

As I sharpen my Python data analysis skills, this dataset serves as both a learning opportunity and a gateway to deeper insights about the data job market. My analysis primarily focuses on data science roles, aligning with my career aspirations. The dataset provides rich details on job postings over the past year, including job titles, salaries, locations, required skills, tools, and the companies hiring.

With that, I welcome you to my analysis of the data job market, focusing on trends within data science roles.

# Key Questions

___________________________________________________________________________________
Here are the questions I will try to tackle in my project:

1. What are the skills in-demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Scientists?
3. How well do jobs and skills pay for Data Scientist?
4. What are the optimal skills for data scientists to learn? (High Demand & High Paying)

# Tools I used

___________________________________________________________________________________
For my investigative analysis of the data job market in the US, I leverage several key tools:

* _Python_: The backbone of my analysis, allowing me to analyze the data and find critical insights. Within Python, I use the following libraries:
  * _Pandas_:Essential for data manipulation and analysis.
  * _Matplotlib_:  I use this for basic data visualization.
  * _Seaborn_: I employ this to create more aesthetically pleasing and advanced visualisations.
* _Jupyter Notebooks_: I used this tool to run my Python scripts, taking advantage of the cells because I find it most suitable to handle data analysis while incorporating my notes.
* _Visual Studio Code_: My default IDE for executing Python scripts because it's easy to use, it has an abundance of extensions and it's free and open source.
* _Git & Github_: These are essential for version control and sharing my Python code and analysis, ensuring collaboration, project tracking and valuable peer reviews.

# Data Preparation and Clean-up

___________________________________________________________________________________
Within this section, I will outline the steps I take to prepare and clean my data to ensure accuracy.

### Importing and Cleaning up Data

___________________________________________________________________________________
I begin by importing the necessary libraries and loading the dataset. For my initial clean up, I correct the date format and the format of the data in the job_skills column for usability. Finally, I set a default uniform aesthetic for my future visualizations using seaborn.

``` 
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from datasets import load_dataset

# Loading Data 
dataset = load_dataset("lukebarousse/data_jobs")
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)

# Set Universal Theme
sns.set_theme(context='notebook', style='darkgrid', palette='deep', font='sans-serif', font_scale=1, color_codes=True, rc=None)
```

### Filter for US Jobs

___________________________________________________________________________________
Next, I filter for just job roles based in the US because they have the most data available with much less missing values so my analysis would be much more accurate.

`df_us = df[df['job_country'] == 'United States']`

# Analysis

___________________________________________________________________________________
Each Jupyter notebook for this project is aimed at investigating specific aspects of the data job market. Here is my approach to each question:

## 1. What are the most demeanded skills for the top 3 most popular data roles?

To identify the most in-demand skills for the top three popular data roles, I begin by filtering the positions by popularity and then pinpointed the top five skills associated with each role. This analysis highlights the leading job titles and their essential skills, helping me determine which abilities to prioritize as I work toward my goal of becoming a Data Scientist.

You can view the detailed steps here: [2_Skills_Demand.](3_Project\2_Skills_Demand.ipynb)

### Visualizing Data

```
fig, axes = plt.subplots(3, 1, figsize=(10, 15))

for i, job_title in enumerate(job_titles):
  df_plt = df_skills_percent[df_skills_percent['job_title_short'] == job_title].head(5)
  #df_plt.plot(kind='barh', x='job_skills', y='skill_percent', ax=axes[i], title=job_title)
  sns.barplot(data=df_plt, x='skill_percent', y='job_skills', ax=axes[i], hue='skill_count')
  axes[i].set_title(job_title)
  #axes[i].invert_yaxis() 
  axes[i].set_ylabel('')
  axes[i].set_xlabel('')
  axes[i].legend().set_visible(False)
  axes[i].set_xlim(0, 80)

  # using the .text function to input the percentage figures right inside the gragh
  for n, v in enumerate(df_plt['skill_percent']):  # n for index and v for value
    axes[i].text(v + 1, n, f'{v:.0f}&', va='center') # +1 gives a space between the bar and the number.

  # Now to remove the xticks for all but the last plot
  if i != len(job_titles) -1: # Identify the position of the plot
    axes[i].set_xticks([]) # Setting to an empty list leaves the xticks empty


fig.suptitle('Percentage of Top Skills in Data Job Postings in the US', fontsize=22)
#fig.tight_layout(h_pad=2)
plt.show()

```

### Results

![Bar chart showing the percentage of a skill showing up on a job posting for any of the top 3 Data job postings in the US in 2023](3_Project\Images\Percentage_of_Top_Skills_in_Data_Job_Postings_in_the_US.png)

### Insights

This chart breaks down the most in-demand skills for three different roles: Data Analyst, Data Engineer, and Data Scientist.

* Data Analysts: SQL is the most popular skill, required in 51% of job postings. Excel follows at 41%, which makes sense for analysts since Excel is widely used for data manipulation. Tableau and Python come next at around 28% and 27%, reflecting the need for visualization and programming skills. SAS is mentioned in 19% of the postings, mainly for more statistical work.

* Data Engineers: SQL and Python are almost equally important here, at 68% and 65%, showing how crucial it is to handle databases and code for data infrastructure. AWS (43%), Azure (32%), and Spark (32%) are highly relevant for cloud and big data platforms, indicating the growing need for engineers to work with large-scale data systems.

* Data Scientists: Python dominates at 72%, highlighting its key role in machine learning, data analysis, and visualization. SQL is also important at 51%, necessary for managing databases. R appears at 44%, often used for statistical analysis and data modeling. Both SAS and Tableau are mentioned in 24% of postings, though they seem to be secondary to Python and R.

## 2. How are in_demand skills trending for Data Scientists in 2023?

To analyze how skills for Data Scientists trended over the past year, I filter the Data Scientist positions and group the skills by the month of the job postings. This approach allow me to identify the top five skills for Data Scientists each month, revealing how the popularity of these skills trended throughout the year.

The detailed steps can be found here, [3_Skills_Trend.](3_Project\3_Skills_Trend.ipynb)

### Visualizing Data

```
plt.figure(figsize=(8, 4))

sns.lineplot(data=df_plot, dashes=False)
plt.title('Top Trending Skills for Data Scientists in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

# Format the y-axis as a percentage and add text labels for each line
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(12, df_plot.iloc[-1, i], df_plot.columns[i], ha='left', va='center')

plt.show()

```

### Results

![Line chart showing how the demand for each skill trended throughout the year](3_Project\Images\Top_Trending_Skills.png)

### Insights

This chart shows how the demand for different skills has changed over 2023 for Data Scientists.

* Python: While still the most in-demand skill, its trend shows a slight decline over the year, dipping below 70% towards the end. However, it remains essential for most data scientist roles.

* SQL: The demand for SQL has remained relatively stable throughout the year, hovering around 50%. It shows slight dips during certain months, but overall, it continues to be crucial for handling databases.

* R: There’s a gradual decline in the demand for R, dropping from around 45% at the beginning of the year to below 40% by the end. This might reflect the shift towards Python, which has become more versatile in recent years.

* SAS and Tableau: Both skills have maintained a relatively lower demand, hovering around 20%. They are valuable in specific sectors but aren’t as broadly required compared to Python and SQL.

These insights suggest that focusing on Python and SQL would provide the most benefit for aspiring Data Scientists, while tools like R, SAS, and Tableau can be useful for specific roles but may not be as widely in demand.

## 3. How well do jobs and skills pay for Data Scientists?

To identify the highest paying roles and skills, I filter for job postings in the United States and their median salary. But first I look at the salary distributions of common data jobs like Data Analysts and Data Engineers to get an idea of which jobs tend to earn more.

You can check that out here, [4_Salary_Analysis.](3_Project\4_Salary_Analysis.ipynb)

### Visualizing Data

```
plt.figure(figsize=(8, 6))
sns.boxplot(data=top_6, x='salary_year_avg', y='job_title_short', order=job_order, palette='deep', hue='job_title_short')
plt.xlabel('Yearly Salary in USD')
plt.ylabel('')  # Set y-axis label
plt.title('Distribution of Data Jobs Average Salaries in the US')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}k') # Create a varible to format the salary
plt.gca().xaxis.set_major_formatter(ticks_x) #Use the varible with the set_major_formatter method
plt.xticks(rotation=45, ha="right")  # Rotate x-axis labels for better readability
plt.show()
```

### Results

![Boxplot showing the distribution of salary among the top data jobs in the US in 2023](3_Project\Images\Boxplot_Distrubution_of_Top_Data_Jobs_in_the_US.png)

### Insights

This chart shows how much different data jobs pay in the US. Here are some key takeaways:

* **Senior roles pay more**: Senior Data Scientists and Senior Data Engineers generally earn higher salaries than their non-senior counterparts.

* **Wide salary ranges**: Each job has a big range of salaries, shown by the long boxes and lines. This means pay can vary a lot within the same job title.

* **Data Scientists earn well**: Both regular and senior Data Scientists seem to have high earning potential, with some salaries reaching past $400,000.

* **Data Analysts at the lower end**: Data Analysts tend to have lower salaries compared to other roles, but there's still room for growth.

* **Outliers exist**: Those little circles far from the boxes are outliers - unusual cases where people earn much more or less than typical.

* **Engineering vs. Science**: Data Engineering and Data Science roles seem pretty close in pay, with neither consistently outearning the other.

* **Career progression**: You can see a path from Data Analyst to Data Scientist to Senior Data Scientist, with salaries generally increasing along that path.

This chart gives a good snapshot of what people in data jobs might expect to earn, though individual salaries can vary based on factors like location, company size, and experience.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrow my analysis, focusing only on Data Scientist roles. I looked at the highest paid skills and the most in_demand skills. I use two bar charts to showcase these.

### Visualizing Data

```
fig, axes = plt.subplots(2, 1, figsize=(10, 8))   # Adjust figsize

sns.barplot(data=top_paying_skills, x='median', y=top_paying_skills.index, ax=axes[0], hue='median')
axes[0].legend().remove()
axes[0].set_xlabel('')
axes[0].set_ylabel('')  # Set y-axis label
axes[0].set_title('Top 10 Highest Paying Data Scientist Skills in the US')
axes[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}k'))

sns.barplot(data=top_trending_skills, x='median', y=top_trending_skills.index, ax=axes[1], hue='median')
axes[1].legend().remove()
axes[1].set_xlabel('Average Yearly Salary in USD')
axes[1].set_ylabel('')  # Set y-axis label
axes[1].set_title('Top 10 Most Demanded Data Scientist Skills in the US')
axes[1].set_xlim(axes[0].get_xlim())
axes[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}k'))

plt.tight_layout()
plt.show()
```

### Results

![Chart showing the top highest paying vs most demanded skills for data scientists in the US in 2023](3_Project\Images\Duoble_Barh_plot_for_Investigative_Salary_Analysis.png)

### Insights

This first chart lists the skills that command the highest salaries in the data science field while the second chart lists the skills that are most frequently mentioned in job postings, along with their associated average salaries. Here's what stands out:

* Asana, Airtable, Watson, Unreal: These tools, primarily known for project management, collaborative software, and AI, offer some of the highest salaries, pushing towards $250k annually. While these may not be the typical skills associated with data science, they appear to be highly valued in specialized or niche roles.
* Python, SQL, R: These are the foundational skills in data science, appearing consistently at the top of most job postings. The salaries hover around $100k, reflecting their universal necessity in the field.
* For high-paying roles, mastering less common but valuable skills like Asana, Watson, or Hugging Face could help you stand out, especially if you're aiming for specialized data science positions.
* For most demanded skills, focusing on Python, SQL, and R is essential, as these are the bread and butter of data science. Adding tools like AWS, TensorFlow, or Spark can boost your job prospects and salary, as they combine high demand with advanced technical capabilities.

## 4. What are the most optimal skills to learn for Data Science?

In order to identify the most optimal skills to learn, ideally, we would need to be informed on the most demanded and the most valuable skill in terms of salary. For this, I calculate the percentage of skill demand and the median salary offered for these skills in 2023. That way, deciding a skill to master would be much easier because I can compare the demand of the skill and it's median salary.

To make things more detailed, I colour-code the skills based on which technological group they belong.

The detailed steps can be found here, [5_Optimal_Skills](3_Project\5_Optimal_Skills.ipynb)

### Visualizing Data

```
# plotting
plt.figure(figsize=(8, 6))
sns.scatterplot(
    data=df_plot, 
    x='skills_percent', 
    y='average_pay',
    hue='Technology',
    alpha=0.9
)

texts = []
# Loop through the index of df_skills to get the appropriate text label
for index, txt in enumerate(df_skills.index):
    texts.append(plt.text(df_skills['skills_percent'].iloc[index], df_skills['average_pay'].iloc[index], txt))

# Inserting arrows to avoid text overlap and for better readability
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

# Add labels and title
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}k'))
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{y:.0f}%'))
plt.xlabel('Percentage of Data Scientist jobs')
plt.ylabel('Average Yearly Salary')
plt.title('Most In-Demand Skills for Data Scientists in the US')
plt.show()
```

### Results

![Scatter plot highlighting the most popular data science skill and their median pay in the US in 2023](3_Project\Images\Scatter_Plot_Most_In-Demand_Skills_For_Data_Scientist.png)

### Insights

This chart highlights the key skills in demand for Data Scientists in the US, showing how they tie into both salary and job availability. Here’s what stands out:

* **Python leads the way**: It’s required in over 70% of Data Scientist jobs, making it the most popular skill. It also comes with a solid salary around $132,000.

* **SQL follows closely**: Needed in about 55% of jobs, SQL is just as crucial and offers a comparable salary at around $135,000.

* **Machine learning libraries pay off**: PyTorch and TensorFlow, despite being required in fewer jobs (around 10%), offer the highest salaries—around $150,000.

* **Cloud skills are valuable**: AWS is in demand for about 20% of positions, and it comes with a strong salary of around $134,000.

* **Traditional tools still matter**: Excel and Tableau, while required in 10-20% of jobs, offer respectable salaries of around $125,000.

* **R is holding steady**: It’s used in about 45% of roles and provides a salary of around $126,000.

* **Big data tools are key**: Spark and Hadoop show up in 15-20% of job postings, with Spark offering a higher average salary of around $136,000.

* **A wide range of skills is necessary**: The chart shows a mix of programming languages, analysis tools, cloud platforms, and machine learning libraries, underlining the diverse skillset required for Data Scientists.

* **Rarer skills mean higher pay**: Skills like PyTorch and TensorFlow, though less commonly required, tend to come with higher salaries, likely because they’re harder to find.

This gives a clear picture of which skills to focus on if you're aiming for any Data role—balancing job opportunities with salary potential.

# Project Insights

__________________________________________________________________________________
This project revealed several important insights about the data job market, especially for data-focused roles:

* **Skill Demand and Salary Connection**: There’s a strong link between the demand for certain skills and the salaries they offer. Core and advanced skills like Python and machine learning libraries tend to command higher salaries, while more specialized tools like TensorFlow or AWS can give you an edge.

* **Shifting Market Trends**: The demand for skills is constantly evolving, and staying updated on these trends is key to remaining competitive. For example, while foundational tools like SQL and Excel remain essential, newer technologies like cloud platforms and machine learning frameworks are becoming more valuable.

* **Maximizing Economic Value**: Knowing which skills are both in high demand and well-compensated is crucial. Focusing on learning skills that balance job availability and pay—like Python, AWS, and Spark—can help data professionals plan their learning path strategically to increase their career and salary potential.

Overall, understanding these insights provides a solid roadmap for anyone aiming to grow in the data industry.

# Challenges

__________________________________________________________________________________
This project came with its fair share of challenges, each offering valuable learning experiences:

* **Data Inconsistencies**: Dealing with missing or inconsistent data was a significant hurdle. It taught me the importance of thorough data cleaning to maintain the integrity of my analysis and ensure that the insights I derived were accurate.

* **Complex Data Visualization**: Creating clear and impactful visualizations from complex data was another challenge. It’s not just about making charts—it’s about presenting the data in a way that’s easy to understand while still highlighting key insights.

* **Balancing Breadth and Depth**: One of the trickiest parts was deciding how deeply to dive into specific analyses without losing sight of the bigger picture. Finding the right balance between covering a wide range of topics and providing meaningful detail was crucial to making sure the project stayed focused and comprehensive.

Each of these challenges pushed me to refine my approach and helped me grow both technically and strategically in my analysis work.

# Personal Takeaways

__________________________________________________________________________________
Through this project, I gained a deeper understanding of the data job market and sharpened my technical skills in Python, especially in working with data. Here are a few key things I learned:

* **Advanced Python Skills**: I became more proficient with libraries like Pandas for manipulating data, and Seaborn and Matplotlib for creating visualizations. These tools helped me handle and analyze data more effectively, making the process smoother and more insightful.

* **The Value of Data Cleaning**: I realized how essential it is to clean and prepare data thoroughly before diving into any analysis. This step ensures that the insights you get are accurate and reliable, which is a critical lesson in handling real-world data.

* **Skill Demand and Strategy**: This project really showed me how important it is to align my skills with what's in demand in the job market. Knowing how skill demand, salary, and job availability are connected will help me make smarter decisions as I plan my career in data science.

All in all, this project was a great way to strengthen both my data analysis skills and my understanding of the job market I’m stepping into.

# Conclusion

__________________________________________________________________________________
As I wrap up this project, it's clear that diving into the data job market isn’t just about crunching numbers, but more about about telling a story with them. From identifying the most sought-after skills to realizing how salary and demand intertwine, I’ve gained insights that will guide my own journey into data science.

The project was not without its bumps; missing data, tricky visualizations, and that constant tug-of-war between going deep or staying broad. But each challenge was an opportunity to sharpen my skills and push myself further. I’ve learned that mastering Python isn’t just a skill to list on a résumé, but it’s a tool to navigate the unpredictable waters of the job market.

As for what comes next, let’s just say I’m more prepared for the twists and turns of this ever-evolving industry. And if I’ve learned anything, it’s that the key to success in data (and maybe life?) is simple: stay curious, keep learning, and always be ready to pivot when the trends shift.

In short, the data doesn’t lie, unless it’s messy. Then, it’s my job to clean it up.
