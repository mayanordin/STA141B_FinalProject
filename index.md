# Economic Impact of Job Market Trends and Work Models  

**STA 141B Final Project**  
**Professor:** Peter Kramlinger  
**Date:** December 11th, 2024  
**By:** Maya Nordin and Ruba Thekkath  
![Image for Title](https://www.talentlms.com/blog/wp-content/uploads/2022/09/remote-work-vs-work-from-home.png)

---
### I. Abstract
<div align="justify">
This paper explores the economic implications of remote work and hybrid models in the contemporary job market. Our research involves collecting data through web scraping and API integration from prominent job platforms such as LinkedIn and Indeed. By analyzing trends across various industries, we aim to assess how the demand for specific roles evolves within these sectors. Key areas of focus include salary distribution, geographic location, job industry, and the frequency of job postings, with a comparative lens on remote, hybrid, and in-office roles. To convey our findings effectively, we will employ data visualizations, including maps highlighting regions where remote and hybrid work are prevalent and graphs illustrating trends in salary, location, and job frequency. Through meticulous data organization and analysis, this study seeks to provide actionable insights into the economic trends driven by evolving work models. Our findings will contribute to a deeper understanding of the shifting employment landscape in a post-pandemic era.
</div>

### II. Introduction
<div align="justify">
The shifting dynamics of the job market occur continuously, redefining traditional employment structures and influencing job postings, hiring practices, and salary trends. These changes significantly impact the labor market, reshaping job opportunities and economic patterns across industries and regions. Understanding these trends is crucial, as they influence not only individual roles but also broader economic factors such as regional employment rates and industry specific demand. This study aims to analyze these patterns, providing insights into how the evolving job market is shaping economic growth and workforce dynamics.
</div>

### III. Data Sources
<div align="justify">
The data sets we will be using come from the following job boards: LinkedIn and Indeed. From each job board we collected the job title, the company the job belongs to, location, salary, and mode (remote, hybrid or in person). 
</div>

### IV. Methodology
### Web Scraping LinkedIn
<div align="justify">
To collect job data from LinkedIn, our initial approach was to explore the use of an API. However, we quickly realized that LinkedIn's API is exclusively available for professional business purposes, limiting its accessibility for projects like ours. With this limitation in mind, we decided to attempt web scraping as an alternative method to gather the required data.
<br><br>
Our initial strategy for web scraping involved manual extraction. Using the "Inspect" option in Chrome, we attempted to access the HTML structure of LinkedIn's job postings. This would allow us to identify and extract specific XPaths corresponding to key job details such as job title, location, salary, and work mode (e.g., remote or hybrid). However, we found LinkedIn's strict Terms of Service explicitly prohibit web scraping. Continuing with this approach posed a risk of violating their policies, potentially resulting in our IP address being banned or other penalties.
<br><br>
Recognizing these risks, we had to reconsider our data collection strategy to ensure compliance with LinkedIn’s guidelines while still achieving our project goals. This setback highlighted the importance of adhering to ethical and legal practices when dealing with online platforms.
<br><br>
To address our data collection needs, we utilized Selenium and Beautiful Soup for web scraping. Selenium enabled us to launch and control its own Chrome browser to interact with LinkedIn, while Beautiful Soup served as an HTML parser to extract the desired information. To begin, we downloaded the appropriate Chrome WebDriver and saved it to our desktop for integration with Selenium.
<br><br>
Next, we navigated to LinkedIn and conducted job searches using the platform's toolbar. Our focus was on collecting data from specific industries within the United States, including Financial Services, Software Development, IT Services and Consulting, Hospitals and Health, Retail, Staffing and Recruiting, Truck Transportation, Construction, Civil Engineering, and Medical Practices. For each industry, we applied filters to target jobs with a minimum salary of $40,000, across all experience levels, and in any work mode (remote, hybrid, or in-person).
<br><br>
We generated a total of 10 unique URLs, one for each industry, and used the Selenium WebDriver to open a browser instance for each link. From there, we iteratively scraped the job listings on each page. On average, each industry provided between 500 and 900 job postings. After consolidating the data from all industries, we successfully compiled a dataset containing 8,223 rows of job information. This took about 9 hours to get all the data as when using the Chrome Driver we needed to manually scroll through all the jobs so the website downloaded, meaning we could not leave the code running on its own. This process ensured we captured a broad and diverse range of job data, laying the groundwork for further analysis and insights into industry-specific trends. 
<br><br>
After retrieving the data, we utilized Pandas in order to store the data as a data frame with job title, job name, job location, job salary and job mode. This data frame would be used to merge with Indeed for further cleaning and plotting. 
</div>

### Web Scraping Indeed 

<div align="justify">
In order to scrape Indeed overall, we utilized Selenium to automate the process of extracting job listings from the website. Initially, we attempted manual scraping by directly accessing the website's HTML structure to collect data, but this approach proved ineffective due to dynamic content loading and anti-scraping measures that blocked our efforts. 
<br><br>
As a result, we pivoted to using Selenium, which allowed us to set up a Chrome browser instance through the Selenium WebDriver. This automated browser navigates to a specified URL and collects data on job postings, including the company name, job location, job title, and salary information. For each job listing, we extracted details using HTML class names and CSS selectors, while also implementing exception handling for scenarios where certain elements, such as salary information, were missing. This method proved to be more reliable than manual scraping, though it still presented significant challenges due to website restrictions. 
<br><br>
We also wanted to add a level of complexity and developed code for pagination, identifying and navigating to the next page of listings until no more pages remain. After this, we searched for jobs on Indeed using the toolbar and collected jobs from the following industries; Financial services, Software Development, IT services and IT consulting, Hospitals and Health, Retail, Staffing and Recruiting, Truck Transportation, Construction, Civil Engineering and Medical Practices. We then searched each of these industries individually and filtering in the search bar such that each job had a salary of $40,000+ and were of any level as well as any mode (remote, hybrid, or in person). 
<br><br>
On top of that, to make sure we could add variety in terms of the jobs we were scraping, we made code to randomly generate URL links that were combinations of different areas, jobs, and salary ranges. Then, we used those links as the link that the code would use to fetch and scrape data. The collected data is stored in a structured format using Python dictionaries and ultimately compiled into a pandas DataFrame. Finally, the script ensures efficient resource management by closing the browser once the process is complete, and we collected a total of 5,574 rows. 
</div>

### Combining and Cleaning Data Set 

<div align="justify">
In order to combine the dates, we ensured that we had the same exact column names, ordering, and data types, and merged the datasets. Then, since we were scraping popular job boards, we wanted to make sure there were no duplicates, so after filtering for duplicates, we had a total of 10,234 rows of data. 
</div>
<br><br>
<div align="center">
    <img width="1164" alt="Original Data Pre-Clean" src="https://github.com/user-attachments/assets/4de260a1-2b7c-4ef7-afca-ef717cdf5c53">
    <p><b>Figure 1:</b> Original Dataset after web scraping, cleaning, & merging </p>
</div>

<div align="center">
    <img width="1164" alt="Original Data Pre-Clean" src="https://github.com/user-attachments/assets/0cea2a78-d3ae-4124-be64-b048786ee7e2">
    <p><b>Figure 2:</b> Final Dataset </p>
</div>

### Salary

<div align="justify">
    
In our initial dataset, the salary information column was unstructured and required significant cleaning. Salaries were provided in ranges, such as "$131.4K/yr - $154.4K/yr" or "$20/hr - $30/hr." Additionally, some job entries included non-salary components, such as "401K" or "Medical/Vision" benefits. The salary information was stored as strings rather than numeric data, which complicated quantitative analysis. To focus solely on salary for comparing industries and job roles, we developed a function to extract numerical data. For salary ranges, the lower and upper bounds were recorded as numerical values (e.g., 131.4 and 154.4 for the example above). For hourly salaries, we converted the values to annual estimates using the formula:
<br><br>
Yearly Salary = Hourly Rate 40 hours/week x 52 weeks/year
<br><br>
We added two new columns: ‘Lower Range’ and ‘Upper Range’, corresponding to the minimum and maximum salaries. We then computed the mean salary for each job entry by averaging these two values and stored the results in a new column, ‘Mean Salary’.
<br><br>
<p><b>Figure 3:</b> Histogram of Salary Means </p>
<img width="500" alt="Screenshot 2024-12-10 at 3 31 32 PM" src="https://github.com/user-attachments/assets/ace6a103-b1b0-4e6d-ac52-d159ae7a9786">

To understand the distribution of mean salaries, we plotted a histogram. The distribution was found to be highly right-skewed, indicating the presence of extreme values (outliers) on the higher end of the salary scale. Given this skewness, the median was deemed a more appropriate measure of central tendency than the mean for calculations involving industry or state-level salary averages. Thus, median values were used in subsequent analyses to represent average salaries more accurately.
</div>

### V. Challenges 
### Linkedin 

<div align="justify">
    
The main challenges we faced were through web scraping and getting all the job data. The first issue we had was finding a way to access LinkedIn properly. We were able to get the chrome driver open, and open a separate chrome browser, but in order to use the search option, we needed to log in. First we tried using the guest option and searching through jobs there, but after the jobs loaded it would ask to either login or verify we weren’t a robot. In order to solve this problem, we ended up creating a new LinkedIn account to use in order to web scrape and were able to get through the login by implementing code that allowed for our username and password for the new LinkedIn account to be submitted. 
<br><br>
Another issue we had was getting our data. In the beginning we did not know that with a free linkedIn account, we were limited to 1000 jobs to scrape from each search we made. The pages went from 1 to 40 with 25 jobs on each page. Our aim was to get around 8000 rows. During our first round of getting data, we were able to get around 7000 rows. We then tested to see if there were any duplicates by changing our dictionary of jobs to a data frame. We checked to see if there were any duplicates and when taking away those duplicates (making sure that duplicates counted as the same job title, company, salary, mode and location), our data set came out to be only 25 rows meaning that our function was only scraping jobs from the first page and not moving to the second. To combat this challenge we added a try and except to allow for the computer to go on to the next page. This worked successfully. 
</div>

### Indeed

<div align="justify">
    
Scraping data from Indeed was an incredibly complex and time consuming process due to the strong restrictions imposed by the website. Initially, we tried to manually scrape the page, but given the restrictions that were put up we were unable to do so, so we decided to use Selenium. We encountered major issues with the web driver setup, as the popup browser failed to function correctly, requiring a lot of debugging and repeated adjustments to Chrome versions we had in our computers. Once resolved, we had code that could scrape a single page accurately. 
<br><br>
Then, the next challenge was designing a scraping pipeline capable of navigating through multiple pages efficiently, so that we could cover many more pages. While the code initially worked for individual pages, it frequently ran into CAPTCHA verifications after scraping 2 to 3 pages, which would halt the process entirely. To continue, we had to do a lot of debugging so that we did not have to manually identify the exact page where the scraper stopped, and then start the process all over again by entering the URL and resume scraping from there. From there, we were able to easily collect multiple pages of data without many issues. The combination of technical issues, constant monitoring, and iterative debugging made the task incredibly challenging and required relentless attention to detail to gather our data, but after spending hours on debugging and writing code, we were able to successfully collect over 5000+ rows of data
</div>

### VI. Findings
<p><b>Figure 4:</b> Frequency of Jobs in Each Industry </p>
<div style="text-align: center;">
<img width="1212" alt="Screenshot 2024-12-10 at 11 31 17 AM" src="https://github.com/user-attachments/assets/b937b6f1-b4b9-476e-a465-2f979e698ea6">
</div>

<a href="https://mayanordin.github.io/STA141B_FinalProject/bar_freq_jobs.html">
    <img src="https://img.shields.io/badge/View-Visualization-blue" alt="View Visualization" width="200">
</a>
<br><br>
<p><b>Figure 5:</b> Map of Number of Jobs Per State </p>
<div style="text-align: center;">
<img width="900" alt="Screenshot 2024-12-10 at 11 31 41 AM" src="https://github.com/user-attachments/assets/867719a7-b003-484f-b6a8-a80520d55dde">
</div>

<a href="https://mayanordin.github.io/STA141B_FinalProject/map_job_count.html">
    <img src="https://img.shields.io/badge/View-Visualization-blue" alt="View Visualization" width="200">
</a>
<br><br>
<p><b>Figure 6:</b> Percentage of Job Modes per State </p>
<img width="1291" alt="Screenshot 2024-12-10 at 11 32 05 AM" src="https://github.com/user-attachments/assets/6238ebdc-2d1e-4d96-9ff8-bf0c8ca23d4e">

<a href="https://mayanordin.github.io/STA141B_FinalProject/percent_job_modes.html">
    <img src="https://img.shields.io/badge/View-Visualization-blue" alt="View Visualization" width="200">
</a>
<br><br> 
<p><b>Figure 7:</b> Map of Median Salary per State </p>
<div style="text-align: center;">
<img width="900" alt="Screenshot 2024-12-10 at 1 03 52 PM" src="https://github.com/user-attachments/assets/6518fdc4-12c7-40cc-aacd-246fb5e290fd">
</div>

<a href="https://mayanordin.github.io/STA141B_FinalProject/salary_map-2.html">
    <img src="https://img.shields.io/badge/View-Visualization-blue" alt="View Visualization" width="200">
</a>

<br><br> 
<p><b>Table 1:</b> Top 10 Companies Posting Jobs  </p>

| Rank | Company Name          | Number of Job Postings |
|------|-----------------------|------------------------|
| 1    | PwC                  | 500                    |
| 2    | Walmart              | 285                    |
| 3    | Jobs via Dice        | 265                    |
| 4    | Whole Foods Market   | 165                    |
| 5    | CyberCoders          | 156                    |
| 6    | Deloitte             | 153                    |
| 7    | Class A Drivers      | 149                    |
| 8    | The Job Network      | 139                    |
| 9    | Total Quality Logistics | 115                |
| 10   | Amazon               | 101                    |

---

<p><b>Table 2:</b> Median Salary by Industry   </p>

| Rank | Industry          | Median Salary (k USD) |
|------|-------------------|-----------------------|
| 1    | Law               | 133.1500             |
| 2    | Business          | 122.2000             |
| 3    | Tech              | 120.0000             |
| 4    | Health            | 101.0860             |
| 5    | Other             | 95.0000              |
| 6    | Education         | 88.2232              |
| 7    | Manufacturing     | 71.5000              |
| 8    | Media             | 67.5000              |
| 9    | Retail            | 60.0000              |

---

<p><b>Table 3:</b> Median Salary of Top 10 Companies </p>

| Rank | Company Name          | Median Salary USD (k/yr) |
|------|-----------------------|--------------------------|
| 1    | PwC                  | 182.1500                |
| 2    | Deloitte             | 166.6000                |
| 3    | Amazon               | 160.0500                |
| 4    | Jobs via Dice        | 144.1000                |
| 5    | CyberCoders          | 120.0000                |
| 6    | The Job Network      | 114.7224                |
| 7    | Class A Drivers      | 61.9000                 |
| 8    | Whole Foods Market   | 49.6080                 |
| 9    | Walmart              | 40.5600                 |
| 10   | Total Quality Logistics | 7.5000               |


### VII. Analysis 

<div align="justify">
    
Figure 4 reveals the frequency of job availability across different industries. Industries like technology, business, and healthcare generally have the highest job frequencies, indicating these sectors are driving much of the job market. On the other hand, industries like retail and manufacturing appear to have significantly fewer job listings, suggesting either lower demand or a more stabilized workforce in these sectors. 
<br><br>
Furthermore, Figure 5 shows significant variation in job availability across states. States typically having larger populations, such as California, Texas, and New York, exhibit a high density of job listings, reflecting their robust economies and diverse industries. In contrast, rural or less populous states like Wyoming and Vermont display fewer opportunities, possibly indicating limited industry presence or smaller labor markets. 
<br><br>
Additionally, Figure 6 highlights the proportion of job modes (e.g., remote, hybrid, on-site) in each state. Urban and economically developed states tend to have a higher percentage of remote and hybrid opportunities, likely due to the prevalence of tech-driven industries. Conversely, states with more traditional or labor-intensive industries, such as agriculture or manufacturing, lean towards on-site roles. This shows a clear divide between states embracing flexible work models and those maintaining traditional practices. 
<br><br>
Lastly, Figure 7 indicates that states like California, New York, and Washington offer the highest average salaries, often tied to their concentration of high-paying industries such as tech and finance. Meanwhile, states in the Midwest and South generally offer lower average salaries, reflecting regional economic disparities. Interestingly, some states with a lower cost of living may still provide competitive salaries in niche industries, presenting unique opportunities for job seekers.
<br><br>
Our tables reveal that PwC leads in both job postings and median salary, with 500 listings and a median salary of $182.15k per year, highlighting its strong market presence and competitive compensation. In contrast, Walmart, despite being second in job postings, offers a significantly lower median salary of $40.56k per year, demonstrating variability in job quality among top employers. Additionally, the median salary across industries underscores the dominance of Law, Business, and Tech, with median salaries exceeding $120k, whereas Retail and Media have the lowest median salaries at $60k and $67.5k, respectively.

These findings highlight key labor market trends: industries such as Law and Tech continue to attract top talent through high pay, while high-volume employers like Walmart prioritize volume hiring over competitive compensation. This disparity emphasizes the importance of balancing both job quantity and quality when evaluating employment opportunities.
</div>

### VIII. Conclusions 

<div align="justify">
The data provides valuable insights for various stakeholders in the job market. For job seekers, understanding the frequency of jobs by industry and geographic distribution enables more informed decisions about career paths, relocation, or remote work opportunities, aligning their skills with sectors in high demand. Employers can use these findings to refine hiring strategies, such as offering competitive salaries in regions with lower pay or enhancing flexibility in states with high percentages of remote work to attract top talent. Policymakers can address regional economic disparities by investing in job creation programs for underrepresented industries or promoting remote work infrastructure in less populated areas to bridge workforce gaps. On a societal level, these insights emphasize the growing importance of flexible work models and the role of emerging industries in driving economic growth. By utilizing this information, individuals and institutions can contribute to equitable job access, competitive compensation, and a resilient workforce.
</div>

### IX. Limitations & Further Research 

<div align="justify">
As for limitations, the scope of data used in this study is confined to specific industries, regions, and time periods, which may not fully capture the broader trends across the entire job market. Additionally, job boards such as LinkedIn and Indeed are designed to tailor search results based on factors like location, industry, and user preferences. This could have introduced biases in the data we scraped, as job postings may not reflect the broader, global job market.
For future research, there are several potential directions. Expanding the dataset to include a wider variety of industries, regions, and timeframes could provide a more comprehensive understanding of the trends in remote and hybrid work models. Additionally, collecting data over more frequent time intervals, such as weekly or monthly, would allow for a more detailed analysis of how job market trends evolve over time. Such longitudinal studies could help capture dynamic shifts and emerging patterns that are crucial for understanding long term changes in the labor market.
</div>

---

This project was completed as part of the coursework for STA 141B at UC Davis. The findings reflect the data analyzed and interpretations by Maya Nordin and Ruba Thekkath.


