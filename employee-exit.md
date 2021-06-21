# Clean and Analyze Employee Exit Surveys

In this project, we'll clean and analyze exit surveys from employees of the [Department of Education, Training and Employment (DETE)](https://en.wikipedia.org/wiki/Department_of_Education_and_Training_(Queensland)}) and the Technical and Further Education (TAFE) body of the Queensland government in Australia. The TAFE exit survey can be found [here](https://data.gov.au/dataset/ds-qld-89970a3b-182b-41ea-aea2-6f9f17b5907e/details?q=exit%20survey) and the survey for the DETE can be found [here](https://data.gov.au/dataset/ds-qld-fe96ff30-d157-4a81-851d-215f2a0fe26d/details?q=exit%20survey).

We'll pretend our stakeholders want us to combine the results for *both* surveys to answer the following question:
    
   - Are employees who only worked for the institutes for a short period of time resigning due to some kind of dissatisfaction? What about employees who have been there longer?

# Introduction

First, we'll read in the datasets and do some initial exporation.


```python
#Read in the data
import pandas as pd
import numpy as np
dete_survey = pd.read_csv('dete_survey.csv')

#Quick exploration of the data
pd.options.display.max_columns = 150 # to avoid truncated output 
dete_survey.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>SeparationType</th>
      <th>Cease Date</th>
      <th>DETE Start Date</th>
      <th>Role Start Date</th>
      <th>Position</th>
      <th>Classification</th>
      <th>Region</th>
      <th>Business Unit</th>
      <th>Employment Status</th>
      <th>Career move to public sector</th>
      <th>Career move to private sector</th>
      <th>Interpersonal conflicts</th>
      <th>Job dissatisfaction</th>
      <th>Dissatisfaction with the department</th>
      <th>Physical work environment</th>
      <th>Lack of recognition</th>
      <th>Lack of job security</th>
      <th>Work location</th>
      <th>Employment conditions</th>
      <th>Maternity/family</th>
      <th>Relocation</th>
      <th>Study/Travel</th>
      <th>Ill Health</th>
      <th>Traumatic incident</th>
      <th>Work life balance</th>
      <th>Workload</th>
      <th>None of the above</th>
      <th>Professional Development</th>
      <th>Opportunities for promotion</th>
      <th>Staff morale</th>
      <th>Workplace issue</th>
      <th>Physical environment</th>
      <th>Worklife balance</th>
      <th>Stress and pressure support</th>
      <th>Performance of supervisor</th>
      <th>Peer support</th>
      <th>Initiative</th>
      <th>Skills</th>
      <th>Coach</th>
      <th>Career Aspirations</th>
      <th>Feedback</th>
      <th>Further PD</th>
      <th>Communication</th>
      <th>My say</th>
      <th>Information</th>
      <th>Kept informed</th>
      <th>Wellness programs</th>
      <th>Health &amp; Safety</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Aboriginal</th>
      <th>Torres Strait</th>
      <th>South Sea</th>
      <th>Disability</th>
      <th>NESB</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Ill Health Retirement</td>
      <td>08/2012</td>
      <td>1984</td>
      <td>2004</td>
      <td>Public Servant</td>
      <td>A01-A04</td>
      <td>Central Office</td>
      <td>Corporate Strategy and Peformance</td>
      <td>Permanent Full-time</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>Male</td>
      <td>56-60</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Voluntary Early Retirement (VER)</td>
      <td>08/2012</td>
      <td>Not Stated</td>
      <td>Not Stated</td>
      <td>Public Servant</td>
      <td>AO5-AO7</td>
      <td>Central Office</td>
      <td>Corporate Strategy and Peformance</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>Male</td>
      <td>56-60</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Voluntary Early Retirement (VER)</td>
      <td>05/2012</td>
      <td>2011</td>
      <td>2011</td>
      <td>Schools Officer</td>
      <td>NaN</td>
      <td>Central Office</td>
      <td>Education Queensland</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>Male</td>
      <td>61 or older</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Resignation-Other reasons</td>
      <td>05/2012</td>
      <td>2005</td>
      <td>2006</td>
      <td>Teacher</td>
      <td>Primary</td>
      <td>Central Queensland</td>
      <td>NaN</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>Female</td>
      <td>36-40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Age Retirement</td>
      <td>05/2012</td>
      <td>1970</td>
      <td>1989</td>
      <td>Head of Curriculum/Head of Special Education</td>
      <td>NaN</td>
      <td>South East</td>
      <td>NaN</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>D</td>
      <td>D</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>SA</td>
      <td>SA</td>
      <td>D</td>
      <td>D</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>M</td>
      <td>Female</td>
      <td>61 or older</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
dete_survey.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 822 entries, 0 to 821
    Data columns (total 56 columns):
     #   Column                               Non-Null Count  Dtype 
    ---  ------                               --------------  ----- 
     0   ID                                   822 non-null    int64 
     1   SeparationType                       822 non-null    object
     2   Cease Date                           822 non-null    object
     3   DETE Start Date                      822 non-null    object
     4   Role Start Date                      822 non-null    object
     5   Position                             817 non-null    object
     6   Classification                       455 non-null    object
     7   Region                               822 non-null    object
     8   Business Unit                        126 non-null    object
     9   Employment Status                    817 non-null    object
     10  Career move to public sector         822 non-null    bool  
     11  Career move to private sector        822 non-null    bool  
     12  Interpersonal conflicts              822 non-null    bool  
     13  Job dissatisfaction                  822 non-null    bool  
     14  Dissatisfaction with the department  822 non-null    bool  
     15  Physical work environment            822 non-null    bool  
     16  Lack of recognition                  822 non-null    bool  
     17  Lack of job security                 822 non-null    bool  
     18  Work location                        822 non-null    bool  
     19  Employment conditions                822 non-null    bool  
     20  Maternity/family                     822 non-null    bool  
     21  Relocation                           822 non-null    bool  
     22  Study/Travel                         822 non-null    bool  
     23  Ill Health                           822 non-null    bool  
     24  Traumatic incident                   822 non-null    bool  
     25  Work life balance                    822 non-null    bool  
     26  Workload                             822 non-null    bool  
     27  None of the above                    822 non-null    bool  
     28  Professional Development             808 non-null    object
     29  Opportunities for promotion          735 non-null    object
     30  Staff morale                         816 non-null    object
     31  Workplace issue                      788 non-null    object
     32  Physical environment                 817 non-null    object
     33  Worklife balance                     815 non-null    object
     34  Stress and pressure support          810 non-null    object
     35  Performance of supervisor            813 non-null    object
     36  Peer support                         812 non-null    object
     37  Initiative                           813 non-null    object
     38  Skills                               811 non-null    object
     39  Coach                                767 non-null    object
     40  Career Aspirations                   746 non-null    object
     41  Feedback                             792 non-null    object
     42  Further PD                           768 non-null    object
     43  Communication                        814 non-null    object
     44  My say                               812 non-null    object
     45  Information                          816 non-null    object
     46  Kept informed                        813 non-null    object
     47  Wellness programs                    766 non-null    object
     48  Health & Safety                      793 non-null    object
     49  Gender                               798 non-null    object
     50  Age                                  811 non-null    object
     51  Aboriginal                           16 non-null     object
     52  Torres Strait                        3 non-null      object
     53  South Sea                            7 non-null      object
     54  Disability                           23 non-null     object
     55  NESB                                 32 non-null     object
    dtypes: bool(18), int64(1), object(37)
    memory usage: 258.6+ KB
    


```python
#Read in the data
tafe_survey = pd.read_csv("tafe_survey.csv")

#Quick exploration of the data
tafe_survey.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Record ID</th>
      <th>Institute</th>
      <th>WorkArea</th>
      <th>CESSATION YEAR</th>
      <th>Reason for ceasing employment</th>
      <th>Contributing Factors. Career Move - Public Sector</th>
      <th>Contributing Factors. Career Move - Private Sector</th>
      <th>Contributing Factors. Career Move - Self-employment</th>
      <th>Contributing Factors. Ill Health</th>
      <th>Contributing Factors. Maternity/Family</th>
      <th>Contributing Factors. Dissatisfaction</th>
      <th>Contributing Factors. Job Dissatisfaction</th>
      <th>Contributing Factors. Interpersonal Conflict</th>
      <th>Contributing Factors. Study</th>
      <th>Contributing Factors. Travel</th>
      <th>Contributing Factors. Other</th>
      <th>Contributing Factors. NONE</th>
      <th>Main Factor. Which of these was the main factor for leaving?</th>
      <th>InstituteViews. Topic:1. I feel the senior leadership had a clear vision and direction</th>
      <th>InstituteViews. Topic:2. I was given access to skills training to help me do my job better</th>
      <th>InstituteViews. Topic:3. I was given adequate opportunities for personal development</th>
      <th>InstituteViews. Topic:4. I was given adequate opportunities for promotion within %Institute]Q25LBL%</th>
      <th>InstituteViews. Topic:5. I felt the salary for the job was right for the responsibilities I had</th>
      <th>InstituteViews. Topic:6. The organisation recognised when staff did good work</th>
      <th>InstituteViews. Topic:7. Management was generally supportive of me</th>
      <th>InstituteViews. Topic:8. Management was generally supportive of my team</th>
      <th>InstituteViews. Topic:9. I was kept informed of the changes in the organisation which would affect me</th>
      <th>InstituteViews. Topic:10. Staff morale was positive within the Institute</th>
      <th>InstituteViews. Topic:11. If I had a workplace issue it was dealt with quickly</th>
      <th>InstituteViews. Topic:12. If I had a workplace issue it was dealt with efficiently</th>
      <th>InstituteViews. Topic:13. If I had a workplace issue it was dealt with discreetly</th>
      <th>WorkUnitViews. Topic:14. I was satisfied with the quality of the management and supervision within my work unit</th>
      <th>WorkUnitViews. Topic:15. I worked well with my colleagues</th>
      <th>WorkUnitViews. Topic:16. My job was challenging and interesting</th>
      <th>WorkUnitViews. Topic:17. I was encouraged to use my initiative in the course of my work</th>
      <th>WorkUnitViews. Topic:18. I had sufficient contact with other people in my job</th>
      <th>WorkUnitViews. Topic:19. I was given adequate support and co-operation by my peers to enable me to do my job</th>
      <th>WorkUnitViews. Topic:20. I was able to use the full range of my skills in my job</th>
      <th>WorkUnitViews. Topic:21. I was able to use the full range of my abilities in my job. ; Category:Level of Agreement; Question:YOUR VIEWS ABOUT YOUR WORK UNIT]</th>
      <th>WorkUnitViews. Topic:22. I was able to use the full range of my knowledge in my job</th>
      <th>WorkUnitViews. Topic:23. My job provided sufficient variety</th>
      <th>WorkUnitViews. Topic:24. I was able to cope with the level of stress and pressure in my job</th>
      <th>WorkUnitViews. Topic:25. My job allowed me to balance the demands of work and family to my satisfaction</th>
      <th>WorkUnitViews. Topic:26. My supervisor gave me adequate personal recognition and feedback on my performance</th>
      <th>WorkUnitViews. Topic:27. My working environment was satisfactory e.g. sufficient space, good lighting, suitable seating and working area</th>
      <th>WorkUnitViews. Topic:28. I was given the opportunity to mentor and coach others in order for me to pass on my skills and knowledge prior to my cessation date</th>
      <th>WorkUnitViews. Topic:29. There was adequate communication between staff in my unit</th>
      <th>WorkUnitViews. Topic:30. Staff morale was positive within my work unit</th>
      <th>Induction. Did you undertake Workplace Induction?</th>
      <th>InductionInfo. Topic:Did you undertake a Corporate Induction?</th>
      <th>InductionInfo. Topic:Did you undertake a Institute Induction?</th>
      <th>InductionInfo. Topic: Did you undertake Team Induction?</th>
      <th>InductionInfo. Face to Face Topic:Did you undertake a Corporate Induction; Category:How it was conducted?</th>
      <th>InductionInfo. On-line Topic:Did you undertake a Corporate Induction; Category:How it was conducted?</th>
      <th>InductionInfo. Induction Manual Topic:Did you undertake a Corporate Induction?</th>
      <th>InductionInfo. Face to Face Topic:Did you undertake a Institute Induction?</th>
      <th>InductionInfo. On-line Topic:Did you undertake a Institute Induction?</th>
      <th>InductionInfo. Induction Manual Topic:Did you undertake a Institute Induction?</th>
      <th>InductionInfo. Face to Face Topic: Did you undertake Team Induction; Category?</th>
      <th>InductionInfo. On-line Topic: Did you undertake Team Induction?process you undertook and how it was conducted.]</th>
      <th>InductionInfo. Induction Manual Topic: Did you undertake Team Induction?</th>
      <th>Workplace. Topic:Did you and your Manager develop a Performance and Professional Development Plan (PPDP)?</th>
      <th>Workplace. Topic:Does your workplace promote a work culture free from all forms of unlawful discrimination?</th>
      <th>Workplace. Topic:Does your workplace promote and practice the principles of employment equity?</th>
      <th>Workplace. Topic:Does your workplace value the diversity of its employees?</th>
      <th>Workplace. Topic:Would you recommend the Institute as an employer to others?</th>
      <th>Gender. What is your Gender?</th>
      <th>CurrentAge. Current Age</th>
      <th>Employment Type. Employment Type</th>
      <th>Classification. Classification</th>
      <th>LengthofServiceOverall. Overall Length of Service at Institute (in years)</th>
      <th>LengthofServiceCurrent. Length of Service at current workplace (in years)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6.341330e+17</td>
      <td>Southern Queensland Institute of TAFE</td>
      <td>Non-Delivery (corporate)</td>
      <td>2010.0</td>
      <td>Contract Expired</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Neutral</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Neutral</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Face to Face</td>
      <td>-</td>
      <td>-</td>
      <td>Face to Face</td>
      <td>-</td>
      <td>-</td>
      <td>Face to Face</td>
      <td>-</td>
      <td>-</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Female</td>
      <td>26  30</td>
      <td>Temporary Full-time</td>
      <td>Administration (AO)</td>
      <td>1-2</td>
      <td>1-2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6.341337e+17</td>
      <td>Mount Isa Institute of TAFE</td>
      <td>Non-Delivery (corporate)</td>
      <td>2010.0</td>
      <td>Retirement</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>Travel</td>
      <td>-</td>
      <td>-</td>
      <td>NaN</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Disagree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>No</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6.341388e+17</td>
      <td>Mount Isa Institute of TAFE</td>
      <td>Delivery (teaching)</td>
      <td>2010.0</td>
      <td>Retirement</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>NONE</td>
      <td>NaN</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Neutral</td>
      <td>Neutral</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>No</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6.341399e+17</td>
      <td>Mount Isa Institute of TAFE</td>
      <td>Non-Delivery (corporate)</td>
      <td>2010.0</td>
      <td>Resignation</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>Travel</td>
      <td>-</td>
      <td>-</td>
      <td>NaN</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Yes</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>NaN</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6.341466e+17</td>
      <td>Southern Queensland Institute of TAFE</td>
      <td>Delivery (teaching)</td>
      <td>2010.0</td>
      <td>Resignation</td>
      <td>-</td>
      <td>Career Move - Private Sector</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>NaN</td>
      <td>Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Strongly Agree</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>-</td>
      <td>-</td>
      <td>Induction Manual</td>
      <td>Face to Face</td>
      <td>-</td>
      <td>-</td>
      <td>Face to Face</td>
      <td>-</td>
      <td>-</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>Male</td>
      <td>41  45</td>
      <td>Permanent Full-time</td>
      <td>Teacher (including LVT)</td>
      <td>3-4</td>
      <td>3-4</td>
    </tr>
  </tbody>
</table>
</div>




```python
tafe_survey.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 702 entries, 0 to 701
    Data columns (total 72 columns):
     #   Column                                                                                                                                                         Non-Null Count  Dtype  
    ---  ------                                                                                                                                                         --------------  -----  
     0   Record ID                                                                                                                                                      702 non-null    float64
     1   Institute                                                                                                                                                      702 non-null    object 
     2   WorkArea                                                                                                                                                       702 non-null    object 
     3   CESSATION YEAR                                                                                                                                                 695 non-null    float64
     4   Reason for ceasing employment                                                                                                                                  701 non-null    object 
     5   Contributing Factors. Career Move - Public Sector                                                                                                              437 non-null    object 
     6   Contributing Factors. Career Move - Private Sector                                                                                                             437 non-null    object 
     7   Contributing Factors. Career Move - Self-employment                                                                                                            437 non-null    object 
     8   Contributing Factors. Ill Health                                                                                                                               437 non-null    object 
     9   Contributing Factors. Maternity/Family                                                                                                                         437 non-null    object 
     10  Contributing Factors. Dissatisfaction                                                                                                                          437 non-null    object 
     11  Contributing Factors. Job Dissatisfaction                                                                                                                      437 non-null    object 
     12  Contributing Factors. Interpersonal Conflict                                                                                                                   437 non-null    object 
     13  Contributing Factors. Study                                                                                                                                    437 non-null    object 
     14  Contributing Factors. Travel                                                                                                                                   437 non-null    object 
     15  Contributing Factors. Other                                                                                                                                    437 non-null    object 
     16  Contributing Factors. NONE                                                                                                                                     437 non-null    object 
     17  Main Factor. Which of these was the main factor for leaving?                                                                                                   113 non-null    object 
     18  InstituteViews. Topic:1. I feel the senior leadership had a clear vision and direction                                                                         608 non-null    object 
     19  InstituteViews. Topic:2. I was given access to skills training to help me do my job better                                                                     613 non-null    object 
     20  InstituteViews. Topic:3. I was given adequate opportunities for personal development                                                                           610 non-null    object 
     21  InstituteViews. Topic:4. I was given adequate opportunities for promotion within %Institute]Q25LBL%                                                            608 non-null    object 
     22  InstituteViews. Topic:5. I felt the salary for the job was right for the responsibilities I had                                                                615 non-null    object 
     23  InstituteViews. Topic:6. The organisation recognised when staff did good work                                                                                  607 non-null    object 
     24  InstituteViews. Topic:7. Management was generally supportive of me                                                                                             614 non-null    object 
     25  InstituteViews. Topic:8. Management was generally supportive of my team                                                                                        608 non-null    object 
     26  InstituteViews. Topic:9. I was kept informed of the changes in the organisation which would affect me                                                          610 non-null    object 
     27  InstituteViews. Topic:10. Staff morale was positive within the Institute                                                                                       602 non-null    object 
     28  InstituteViews. Topic:11. If I had a workplace issue it was dealt with quickly                                                                                 601 non-null    object 
     29  InstituteViews. Topic:12. If I had a workplace issue it was dealt with efficiently                                                                             597 non-null    object 
     30  InstituteViews. Topic:13. If I had a workplace issue it was dealt with discreetly                                                                              601 non-null    object 
     31  WorkUnitViews. Topic:14. I was satisfied with the quality of the management and supervision within my work unit                                                609 non-null    object 
     32  WorkUnitViews. Topic:15. I worked well with my colleagues                                                                                                      605 non-null    object 
     33  WorkUnitViews. Topic:16. My job was challenging and interesting                                                                                                607 non-null    object 
     34  WorkUnitViews. Topic:17. I was encouraged to use my initiative in the course of my work                                                                        610 non-null    object 
     35  WorkUnitViews. Topic:18. I had sufficient contact with other people in my job                                                                                  613 non-null    object 
     36  WorkUnitViews. Topic:19. I was given adequate support and co-operation by my peers to enable me to do my job                                                   609 non-null    object 
     37  WorkUnitViews. Topic:20. I was able to use the full range of my skills in my job                                                                               609 non-null    object 
     38  WorkUnitViews. Topic:21. I was able to use the full range of my abilities in my job. ; Category:Level of Agreement; Question:YOUR VIEWS ABOUT YOUR WORK UNIT]  608 non-null    object 
     39  WorkUnitViews. Topic:22. I was able to use the full range of my knowledge in my job                                                                            608 non-null    object 
     40  WorkUnitViews. Topic:23. My job provided sufficient variety                                                                                                    611 non-null    object 
     41  WorkUnitViews. Topic:24. I was able to cope with the level of stress and pressure in my job                                                                    610 non-null    object 
     42  WorkUnitViews. Topic:25. My job allowed me to balance the demands of work and family to my satisfaction                                                        611 non-null    object 
     43  WorkUnitViews. Topic:26. My supervisor gave me adequate personal recognition and feedback on my performance                                                    606 non-null    object 
     44  WorkUnitViews. Topic:27. My working environment was satisfactory e.g. sufficient space, good lighting, suitable seating and working area                       610 non-null    object 
     45  WorkUnitViews. Topic:28. I was given the opportunity to mentor and coach others in order for me to pass on my skills and knowledge prior to my cessation date  609 non-null    object 
     46  WorkUnitViews. Topic:29. There was adequate communication between staff in my unit                                                                             603 non-null    object 
     47  WorkUnitViews. Topic:30. Staff morale was positive within my work unit                                                                                         606 non-null    object 
     48  Induction. Did you undertake Workplace Induction?                                                                                                              619 non-null    object 
     49  InductionInfo. Topic:Did you undertake a Corporate Induction?                                                                                                  432 non-null    object 
     50  InductionInfo. Topic:Did you undertake a Institute Induction?                                                                                                  483 non-null    object 
     51  InductionInfo. Topic: Did you undertake Team Induction?                                                                                                        440 non-null    object 
     52  InductionInfo. Face to Face Topic:Did you undertake a Corporate Induction; Category:How it was conducted?                                                      555 non-null    object 
     53  InductionInfo. On-line Topic:Did you undertake a Corporate Induction; Category:How it was conducted?                                                           555 non-null    object 
     54  InductionInfo. Induction Manual Topic:Did you undertake a Corporate Induction?                                                                                 555 non-null    object 
     55  InductionInfo. Face to Face Topic:Did you undertake a Institute Induction?                                                                                     530 non-null    object 
     56  InductionInfo. On-line Topic:Did you undertake a Institute Induction?                                                                                          555 non-null    object 
     57  InductionInfo. Induction Manual Topic:Did you undertake a Institute Induction?                                                                                 553 non-null    object 
     58  InductionInfo. Face to Face Topic: Did you undertake Team Induction; Category?                                                                                 555 non-null    object 
     59  InductionInfo. On-line Topic: Did you undertake Team Induction?process you undertook and how it was conducted.]                                                555 non-null    object 
     60  InductionInfo. Induction Manual Topic: Did you undertake Team Induction?                                                                                       555 non-null    object 
     61  Workplace. Topic:Did you and your Manager develop a Performance and Professional Development Plan (PPDP)?                                                      608 non-null    object 
     62  Workplace. Topic:Does your workplace promote a work culture free from all forms of unlawful discrimination?                                                    594 non-null    object 
     63  Workplace. Topic:Does your workplace promote and practice the principles of employment equity?                                                                 587 non-null    object 
     64  Workplace. Topic:Does your workplace value the diversity of its employees?                                                                                     586 non-null    object 
     65  Workplace. Topic:Would you recommend the Institute as an employer to others?                                                                                   581 non-null    object 
     66  Gender. What is your Gender?                                                                                                                                   596 non-null    object 
     67  CurrentAge. Current Age                                                                                                                                        596 non-null    object 
     68  Employment Type. Employment Type                                                                                                                               596 non-null    object 
     69  Classification. Classification                                                                                                                                 596 non-null    object 
     70  LengthofServiceOverall. Overall Length of Service at Institute (in years)                                                                                      596 non-null    object 
     71  LengthofServiceCurrent. Length of Service at current workplace (in years)                                                                                      596 non-null    object 
    dtypes: float64(2), object(70)
    memory usage: 395.0+ KB
    

We can make the following observations based on the work above:
  - The `dete_survey` dataframe contains `'Not Stated'` values that indicate values are missing, but they aren't represented as `NaN`.
  - Both the `dete_survey` and `tafe_survey` contain many columns that we don't need to complete our analysis.
  - Each dataframe contains many of the same columns, but the column names are different.
  - There are multiple columns/answers that indicate an employee resigned because they were dissatisfied.

# Identify Missing Values and Drop Unneccessary Columns

First, we'll correct the `Not Stated` values and drop some of the columns we don't need for our analysis.


```python
# Read in the data again, but this time read `Not Stated` values as `NaN`
dete_survey = pd.read_csv('dete_survey.csv', na_values='Not Stated')

# Quick exploration of the data
dete_survey.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>SeparationType</th>
      <th>Cease Date</th>
      <th>DETE Start Date</th>
      <th>Role Start Date</th>
      <th>Position</th>
      <th>Classification</th>
      <th>Region</th>
      <th>Business Unit</th>
      <th>Employment Status</th>
      <th>Career move to public sector</th>
      <th>Career move to private sector</th>
      <th>Interpersonal conflicts</th>
      <th>Job dissatisfaction</th>
      <th>Dissatisfaction with the department</th>
      <th>Physical work environment</th>
      <th>Lack of recognition</th>
      <th>Lack of job security</th>
      <th>Work location</th>
      <th>Employment conditions</th>
      <th>Maternity/family</th>
      <th>Relocation</th>
      <th>Study/Travel</th>
      <th>Ill Health</th>
      <th>Traumatic incident</th>
      <th>Work life balance</th>
      <th>Workload</th>
      <th>None of the above</th>
      <th>Professional Development</th>
      <th>Opportunities for promotion</th>
      <th>Staff morale</th>
      <th>Workplace issue</th>
      <th>Physical environment</th>
      <th>Worklife balance</th>
      <th>Stress and pressure support</th>
      <th>Performance of supervisor</th>
      <th>Peer support</th>
      <th>Initiative</th>
      <th>Skills</th>
      <th>Coach</th>
      <th>Career Aspirations</th>
      <th>Feedback</th>
      <th>Further PD</th>
      <th>Communication</th>
      <th>My say</th>
      <th>Information</th>
      <th>Kept informed</th>
      <th>Wellness programs</th>
      <th>Health &amp; Safety</th>
      <th>Gender</th>
      <th>Age</th>
      <th>Aboriginal</th>
      <th>Torres Strait</th>
      <th>South Sea</th>
      <th>Disability</th>
      <th>NESB</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Ill Health Retirement</td>
      <td>08/2012</td>
      <td>1984.0</td>
      <td>2004.0</td>
      <td>Public Servant</td>
      <td>A01-A04</td>
      <td>Central Office</td>
      <td>Corporate Strategy and Peformance</td>
      <td>Permanent Full-time</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>Male</td>
      <td>56-60</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Voluntary Early Retirement (VER)</td>
      <td>08/2012</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Public Servant</td>
      <td>AO5-AO7</td>
      <td>Central Office</td>
      <td>Corporate Strategy and Peformance</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>Male</td>
      <td>56-60</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Voluntary Early Retirement (VER)</td>
      <td>05/2012</td>
      <td>2011.0</td>
      <td>2011.0</td>
      <td>Schools Officer</td>
      <td>NaN</td>
      <td>Central Office</td>
      <td>Education Queensland</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>Male</td>
      <td>61 or older</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Resignation-Other reasons</td>
      <td>05/2012</td>
      <td>2005.0</td>
      <td>2006.0</td>
      <td>Teacher</td>
      <td>Primary</td>
      <td>Central Queensland</td>
      <td>NaN</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>Female</td>
      <td>36-40</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Age Retirement</td>
      <td>05/2012</td>
      <td>1970.0</td>
      <td>1989.0</td>
      <td>Head of Curriculum/Head of Special Education</td>
      <td>NaN</td>
      <td>South East</td>
      <td>NaN</td>
      <td>Permanent Full-time</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>A</td>
      <td>A</td>
      <td>N</td>
      <td>N</td>
      <td>D</td>
      <td>D</td>
      <td>N</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>A</td>
      <td>SA</td>
      <td>SA</td>
      <td>D</td>
      <td>D</td>
      <td>A</td>
      <td>N</td>
      <td>A</td>
      <td>M</td>
      <td>Female</td>
      <td>61 or older</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Remove columns we don't need for our analysis
dete_survey_updated = dete_survey.drop(dete_survey.columns[28:49], axis=1)
tafe_survey_updated = tafe_survey.drop(tafe_survey.columns[17:66], axis=1)

#Check that the columns were dropped
print(dete_survey_updated.columns)
print(tafe_survey_updated.columns)
```

    Index(['ID', 'SeparationType', 'Cease Date', 'DETE Start Date',
           'Role Start Date', 'Position', 'Classification', 'Region',
           'Business Unit', 'Employment Status', 'Career move to public sector',
           'Career move to private sector', 'Interpersonal conflicts',
           'Job dissatisfaction', 'Dissatisfaction with the department',
           'Physical work environment', 'Lack of recognition',
           'Lack of job security', 'Work location', 'Employment conditions',
           'Maternity/family', 'Relocation', 'Study/Travel', 'Ill Health',
           'Traumatic incident', 'Work life balance', 'Workload',
           'None of the above', 'Gender', 'Age', 'Aboriginal', 'Torres Strait',
           'South Sea', 'Disability', 'NESB'],
          dtype='object')
    Index(['Record ID', 'Institute', 'WorkArea', 'CESSATION YEAR',
           'Reason for ceasing employment',
           'Contributing Factors. Career Move - Public Sector ',
           'Contributing Factors. Career Move - Private Sector ',
           'Contributing Factors. Career Move - Self-employment',
           'Contributing Factors. Ill Health',
           'Contributing Factors. Maternity/Family',
           'Contributing Factors. Dissatisfaction',
           'Contributing Factors. Job Dissatisfaction',
           'Contributing Factors. Interpersonal Conflict',
           'Contributing Factors. Study', 'Contributing Factors. Travel',
           'Contributing Factors. Other', 'Contributing Factors. NONE',
           'Gender. What is your Gender?', 'CurrentAge. Current Age',
           'Employment Type. Employment Type', 'Classification. Classification',
           'LengthofServiceOverall. Overall Length of Service at Institute (in years)',
           'LengthofServiceCurrent. Length of Service at current workplace (in years)'],
          dtype='object')
    

# Rename Columns

Next, we'll standardize the names of the columns we want to work with, because we eventually want to combine the dataframes.


```python
# Clean the column names
dete_survey_updated.columns = dete_survey_updated.columns.str.lower().str.strip().str.replace(' ', '_')

# Check that the column names were updated correctly
dete_survey_updated.columns
```




    Index(['id', 'separationtype', 'cease_date', 'dete_start_date',
           'role_start_date', 'position', 'classification', 'region',
           'business_unit', 'employment_status', 'career_move_to_public_sector',
           'career_move_to_private_sector', 'interpersonal_conflicts',
           'job_dissatisfaction', 'dissatisfaction_with_the_department',
           'physical_work_environment', 'lack_of_recognition',
           'lack_of_job_security', 'work_location', 'employment_conditions',
           'maternity/family', 'relocation', 'study/travel', 'ill_health',
           'traumatic_incident', 'work_life_balance', 'workload',
           'none_of_the_above', 'gender', 'age', 'aboriginal', 'torres_strait',
           'south_sea', 'disability', 'nesb'],
          dtype='object')




```python
# Update column names to match the names in dete_survey_updated
mapping = {'Record ID': 'id', 'CESSATION YEAR': 'cease_date', 'Reason for ceasing employment': 'separationtype', 'Gender. What is your Gender?': 'gender', 'CurrentAge. Current Age': 'age',
       'Employment Type. Employment Type': 'employment_status',
       'Classification. Classification': 'position',
       'LengthofServiceOverall. Overall Length of Service at Institute (in years)': 'institute_service',
       'LengthofServiceCurrent. Length of Service at current workplace (in years)': 'role_service'}
tafe_survey_updated = tafe_survey_updated.rename(mapping, axis = 1)

# Check that the specified column names were updated correctly
tafe_survey_updated.columns
```




    Index(['id', 'Institute', 'WorkArea', 'cease_date', 'separationtype',
           'Contributing Factors. Career Move - Public Sector ',
           'Contributing Factors. Career Move - Private Sector ',
           'Contributing Factors. Career Move - Self-employment',
           'Contributing Factors. Ill Health',
           'Contributing Factors. Maternity/Family',
           'Contributing Factors. Dissatisfaction',
           'Contributing Factors. Job Dissatisfaction',
           'Contributing Factors. Interpersonal Conflict',
           'Contributing Factors. Study', 'Contributing Factors. Travel',
           'Contributing Factors. Other', 'Contributing Factors. NONE', 'gender',
           'age', 'employment_status', 'position', 'institute_service',
           'role_service'],
          dtype='object')



#  Filter the Data

For this project, we'll only analyze survey respondents who *resigned*, so we'll only select separation types containing the string `'Resignation'`.


```python
# Check the unique values for the separationtype column
tafe_survey_updated['separationtype'].value_counts()
```




    Resignation                 340
    Contract Expired            127
    Retrenchment/ Redundancy    104
    Retirement                   82
    Transfer                     25
    Termination                  23
    Name: separationtype, dtype: int64




```python
# Check the unique values for the separationtype column
dete_survey_updated['separationtype'].value_counts()
```




    Age Retirement                          285
    Resignation-Other reasons               150
    Resignation-Other employer               91
    Resignation-Move overseas/interstate     70
    Voluntary Early Retirement (VER)         67
    Ill Health Retirement                    61
    Other                                    49
    Contract Expired                         34
    Termination                              15
    Name: separationtype, dtype: int64




```python
# Update all separation types containing the word "resignation" to 'Resignation'
dete_survey_updated['separationtype'] = dete_survey_updated['separationtype'].str.split('-').str[0]

# Check the values in the separationtype column were updated correctly
dete_survey_updated['separationtype'].value_counts()
```




    Resignation                         311
    Age Retirement                      285
    Voluntary Early Retirement (VER)     67
    Ill Health Retirement                61
    Other                                49
    Contract Expired                     34
    Termination                          15
    Name: separationtype, dtype: int64




```python
# Select only the resignation separation types from each dataframe
dete_resignations = dete_survey_updated[dete_survey_updated['separationtype'] == 'Resignation'].copy()
tafe_resignations = tafe_survey_updated[tafe_survey_updated['separationtype'] == 'Resignation'].copy()
```

# Verify the Data

Below, we clean and explore the `cease_date` and `dete_start_date` columns to make sure all of the years make sense. We'll use the following criteria:

   - Since the `cease_date` is the last year of the person's employment and the `dete_start_date` is the person's first year of employment, it wouldn't make sense to have years after the current date. 
   - Given that most people in this field start working in their 20s, it's also unlikely that the `dete_start_date` was before the year 1940.


```python
# Check the unique values
dete_resignations['cease_date'].value_counts()
```




    2012       126
    2013        74
    01/2014     22
    12/2013     17
    06/2013     14
    09/2013     11
    07/2013      9
    11/2013      9
    10/2013      6
    08/2013      4
    05/2012      2
    05/2013      2
    09/2010      1
    07/2006      1
    07/2012      1
    2010         1
    Name: cease_date, dtype: int64




```python
# Extract the years and convert them to a float type
dete_resignations['cease_date'] = dete_resignations['cease_date'].str.split('/').str[-1]
dete_resignations['cease_date'] = dete_resignations['cease_date'].astype("float")

# Check the values again and look for outliers
dete_resignations['cease_date'].value_counts()
```




    2013.0    146
    2012.0    129
    2014.0     22
    2010.0      2
    2006.0      1
    Name: cease_date, dtype: int64




```python
# Check the unique values and look for outliers
dete_resignations['dete_start_date'].value_counts().sort_values()
```




    1963.0     1
    1971.0     1
    1972.0     1
    1984.0     1
    1977.0     1
    1987.0     1
    1975.0     1
    1973.0     1
    1982.0     1
    1974.0     2
    1983.0     2
    1976.0     2
    1986.0     3
    1985.0     3
    2001.0     3
    1995.0     4
    1988.0     4
    1989.0     4
    1991.0     4
    1997.0     5
    1980.0     5
    1993.0     5
    1990.0     5
    1994.0     6
    2003.0     6
    1998.0     6
    1992.0     6
    2002.0     6
    1996.0     6
    1999.0     8
    2000.0     9
    2013.0    10
    2009.0    13
    2006.0    13
    2004.0    14
    2005.0    15
    2010.0    17
    2012.0    21
    2007.0    21
    2008.0    22
    2011.0    24
    Name: dete_start_date, dtype: int64




```python
# Check the unique values
tafe_resignations['cease_date'].value_counts().sort_values()
```




    2009.0      2
    2013.0     55
    2010.0     68
    2012.0     94
    2011.0    116
    Name: cease_date, dtype: int64



Below are our findings:
- The years in both dataframes don't completely align. The `tafe_survey_updated` dataframe contains some cease dates in 2009, but the `dete_survey_updated` dataframe does not. The `tafe_survey_updated` dataframe also contains many more cease dates in 2010 than the `dete_survey_updaed` dataframe. Since we aren't concerned with analyzing the results by year, we'll leave them as is.

# Create a New Column

Since our end goal is to answer the question below, we need a column containing the length of time an employee spent in their workplace, or years of service, in both dataframes.

   - End goal: Are employees who have only worked for the institutes for a short period of time resigning due to some kind of dissatisfaction? What about employees who have been at the job longer?

The `tafe_resignations` dataframe already contains a "service" column, which we renamed to `institute_service`.

Below, we calculate the  years of service in the `dete_survey_updated` dataframe by subtracting the `dete_start_date` from the `cease_date` and create a new column named `institute_service`.


```python
# Calculate the length of time an employee spent in their respective workplace and create a new column
dete_resignations['institute_service'] = dete_resignations['cease_date'] - dete_resignations['dete_start_date']

# Quick check of the result
dete_resignations['institute_service'].head()
```




    3      7.0
    5     18.0
    8      3.0
    9     15.0
    11     3.0
    Name: institute_service, dtype: float64



# Identify Dissatisfied Employees
Next, we'll identify any employees who resigned because they were dissatisfied. Below are the columns we'll use to categorize employees as "dissatisfied" from each dataframe:

   1. tafe_survey_updated:
       - `Contributing Factors. Dissatisfaction`
       - `Contributing Factors. Job Dissatisfaction`
   2.  dafe_survey_updated:
       - `job_dissatisfaction`
       - `dissatisfaction_with_the_department`
       - `physical_work_environment`
       - `lack_of_recognition`
       - `lack_of_job_security`
       - `work_location`
       - `employment_conditions`
       - `work_life_balance`
       - `workload`
    
If the employee indicated any of the factors above caused them to resign, we'll mark them as `dissatisfied` in a new column. After our changes, the new `dissatisfied` column will contain just the following values:

   - `True`: indicates a person resigned because they were dissatisfied in some way
   - `False`: indicates a person resigned because of a reason other than dissatisfaction with the job
   - `NaN`: indicates the value is missing


```python
# Check the unique values
tafe_resignations['Contributing Factors. Dissatisfaction'].value_counts()
```




    -                                         277
    Contributing Factors. Dissatisfaction      55
    Name: Contributing Factors. Dissatisfaction, dtype: int64




```python
# Check the unique values
tafe_resignations['Contributing Factors. Job Dissatisfaction'].value_counts()
```




    -                      270
    Job Dissatisfaction     62
    Name: Contributing Factors. Job Dissatisfaction, dtype: int64




```python
# Update the values in the contributing factors columns to be either True, False, or NaN
def update_vals(x):
    if x == '-':
        return False
    elif pd.isnull(x):
        return np.nan
    else:
        return True
tafe_resignations['dissatisfied'] = tafe_resignations[['Contributing Factors. Dissatisfaction', 'Contributing Factors. Job Dissatisfaction']].applymap(update_vals).any(1, skipna=False)
tafe_resignations_up = tafe_resignations.copy()

# Check the unique values after the updates
tafe_resignations_up['dissatisfied'].value_counts(dropna=False)
```




    False    241
    True      91
    NaN        8
    Name: dissatisfied, dtype: int64




```python
# Update the values in columns related to dissatisfaction to be either True, False, or NaN
dete_resignations['dissatisfied'] = dete_resignations[['job_dissatisfaction',
       'dissatisfaction_with_the_department', 'physical_work_environment',
       'lack_of_recognition', 'lack_of_job_security', 'work_location',
       'employment_conditions', 'work_life_balance',
       'workload']].any(1, skipna=False)
dete_resignations_up = dete_resignations.copy()
dete_resignations_up['dissatisfied'].value_counts(dropna=False)
```




    False    162
    True     149
    Name: dissatisfied, dtype: int64



# Combining the Data

Below, we'll add an institute column so that we can differentiate the data from each survey after we combine them. Then, we'll combine the dataframes and drop any remaining columns we don't need.


```python
# Add an institute column
dete_resignations_up['institute'] = 'DETE'
tafe_resignations_up['institute'] = 'TAFE'
```


```python
# Combine the dataframes
combined = pd.concat([dete_resignations_up, tafe_resignations_up], ignore_index=True)

# Verify the number of non null values in each column
combined.notnull().sum().sort_values()
```




    torres_strait                                            0
    south_sea                                                3
    aboriginal                                               7
    disability                                               8
    nesb                                                     9
    business_unit                                           32
    classification                                         161
    region                                                 265
    role_start_date                                        271
    dete_start_date                                        283
    role_service                                           290
    none_of_the_above                                      311
    work_life_balance                                      311
    traumatic_incident                                     311
    ill_health                                             311
    study/travel                                           311
    relocation                                             311
    maternity/family                                       311
    employment_conditions                                  311
    workload                                               311
    lack_of_job_security                                   311
    career_move_to_public_sector                           311
    career_move_to_private_sector                          311
    interpersonal_conflicts                                311
    work_location                                          311
    dissatisfaction_with_the_department                    311
    physical_work_environment                              311
    lack_of_recognition                                    311
    job_dissatisfaction                                    311
    Contributing Factors. Job Dissatisfaction              332
    Contributing Factors. Travel                           332
    Contributing Factors. Maternity/Family                 332
    Contributing Factors. Ill Health                       332
    Contributing Factors. Career Move - Self-employment    332
    Contributing Factors. Career Move - Private Sector     332
    Contributing Factors. Career Move - Public Sector      332
    Contributing Factors. Dissatisfaction                  332
    Contributing Factors. Other                            332
    Contributing Factors. Interpersonal Conflict           332
    Contributing Factors. NONE                             332
    Contributing Factors. Study                            332
    Institute                                              340
    WorkArea                                               340
    institute_service                                      563
    gender                                                 592
    age                                                    596
    employment_status                                      597
    position                                               598
    cease_date                                             635
    dissatisfied                                           643
    separationtype                                         651
    institute                                              651
    id                                                     651
    dtype: int64




```python
# Drop columns with less than 500 non null values
combined_updated = combined.dropna(thresh = 500, axis =1).copy()
```

# Clean the Service Column 

Next, we'll clean the `institute_service` column and categorize employees according to the following definitions:

 - New: Less than 3 years in the workplace
 - Experienced: 3-6 years in the workplace
 - Established: 7-10 years in the workplace
 - Veteran: 11 or more years in the workplace
 
Our analysis is based on [this article](https://www.businesswire.com/news/home/20171108006002/en/Age-Number-Engage-Employees-Career-Stage), which makes the argument that understanding employee's needs according to career stage instead of age is more effective.


```python
# Check the unique values
combined_updated['institute_service'].value_counts(dropna=False)
```




    NaN                   88
    Less than 1 year      73
    1-2                   64
    3-4                   63
    5-6                   33
    11-20                 26
    5.0                   23
    1.0                   22
    7-10                  21
    0.0                   20
    3.0                   20
    6.0                   17
    4.0                   16
    2.0                   14
    9.0                   14
    7.0                   13
    More than 20 years    10
    8.0                    8
    13.0                   8
    15.0                   7
    20.0                   7
    12.0                   6
    22.0                   6
    17.0                   6
    10.0                   6
    14.0                   6
    18.0                   5
    16.0                   5
    23.0                   4
    24.0                   4
    11.0                   4
    39.0                   3
    21.0                   3
    32.0                   3
    19.0                   3
    36.0                   2
    30.0                   2
    26.0                   2
    28.0                   2
    25.0                   2
    29.0                   1
    31.0                   1
    49.0                   1
    33.0                   1
    34.0                   1
    35.0                   1
    38.0                   1
    41.0                   1
    42.0                   1
    27.0                   1
    Name: institute_service, dtype: int64




```python
# Extract the years of service and convert the type to float
combined_updated['institute_service_up'] = combined_updated['institute_service'].astype('str').str.extract(r'(\d+)')
combined_updated['institute_service_up'] = combined_updated['institute_service_up'].astype('float')

# Check the years extracted are correct
combined_updated['institute_service_up'].value_counts()
```




    1.0     159
    3.0      83
    5.0      56
    7.0      34
    11.0     30
    0.0      20
    20.0     17
    6.0      17
    4.0      16
    9.0      14
    2.0      14
    13.0      8
    8.0       8
    15.0      7
    17.0      6
    10.0      6
    12.0      6
    14.0      6
    22.0      6
    16.0      5
    18.0      5
    24.0      4
    23.0      4
    39.0      3
    19.0      3
    21.0      3
    32.0      3
    28.0      2
    36.0      2
    25.0      2
    30.0      2
    26.0      2
    29.0      1
    38.0      1
    42.0      1
    27.0      1
    41.0      1
    35.0      1
    49.0      1
    34.0      1
    33.0      1
    31.0      1
    Name: institute_service_up, dtype: int64




```python
# Convert years of service to categories
def transform_service(val):
    if val >= 11:
        return "Veteran"
    elif 7 <= val < 11:
        return "Established"
    elif 3 <= val < 7:
        return "Experienced"
    elif pd.isnull(val):
        return np.nan
    else:
        return "New"
combined_updated['service_cat'] = combined_updated['institute_service_up'].apply(transform_service)

# Quick check of the update
combined_updated['service_cat'].value_counts()
```




    New            193
    Experienced    172
    Veteran        136
    Established     62
    Name: service_cat, dtype: int64



# Perform Some Initial Analysis 

Finally, we'll replace the missing values in the `dissatisfied` column with the most frequent value, `False`. Then, we'll calculate the percentage of employees who resigned due to dissatisfaction in each `service_cat` group and plot the results.

Note that since we still have additional missing values left to deal with, this is meant to be an initial introduction to the analysis, *not* the final analysis.


```python
# Verify the unique values
combined_updated['dissatisfied'].value_counts(dropna=False)
```




    False    403
    True     240
    NaN        8
    Name: dissatisfied, dtype: int64




```python
# Replace missing values with the most frequent value, False
combined_updated['dissatisfied'] = combined_updated['dissatisfied'].fillna(False)
```


```python
# Calculate the percentage of employees who resigned due to dissatisfaction in each category
dis_pct = combined_updated.pivot_table(index='service_cat', values='dissatisfied')

# Plot the results
%matplotlib inline
dis_pct.plot(kind='bar', rot=30)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7f6ef8b9c760>




    
![png](output_44_1.png)
    


From the initial analysis above, we can tentatively conclude that employees with 7 or more years of service are more likely to resign due to some kind of dissatisfaction with the job than employees with less than 7 years of service. However, we need to handle the rest of the missing data to finalize our analysis.
