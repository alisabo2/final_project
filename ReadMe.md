# # SISE2601 Final Project in Advanced Data Analysis written in R
### _**Research question: The relationship between the characteristics of patient entry and the treatment that was given to the success in rehab**_

This project aims to create a Decision Support System (DSS) based on predictions about the improvement of patients in a treatment program. The goal is to identify the most successful treatments that yield the best improvement for patients. The code provided implements the DSS and follows several steps outlined below.

_**Team members: Alisa Bogomolny, Elore Paz, Dor Meir**_

----------------------------------------------------------------------------------------------------------------------
# Steps Of Our Project:

## Step 1: Data Collection and Preparation

The necessary data is found in this [link](https://www.datafiles.samhsa.gov/sites/default/files/field-uploads-protected/studies/TEDS-D-2018/TEDS-D-2018-datasets/TEDS-D-2018-DS0001/TEDS-D-2018-DS0001-bundles-with-study-info/TEDS-D-2018-DS0001-bndl-data-r.zip).
This step involves gathering historical patient data, including demographic information, medical history, treatment details, and corresponding improvement measurements. 
The collected data is then organized and preprocessed to ensure its quality and suitability for analysis.
The preprocessing involves:
1. Cleaning the data set record by removing all the patients that don't finished the treatment. the feature we have filtered is REASON and it is described below.

|Reason |Treatment Combination | 
|--|------------------------------------------------------|
|1 |Treatment completed |
|2 |Dropped out of treatment | 
|3 |Terminated by facility |
|4 |Transferred to another treatment program or facility |
|5 |Incarcerated |
|6 |Death |
|7 |Other |

2. Removing the following features- "DISYR", "SERVICES_D", "REASON","EMPLOY_D", "LIVARAG_D", "ARRESTS_D, DETNLF_D", "FREQ_ATND_SELF_HELP_D". this features are related to the discharge attributes and this information don't help us in the process.

3. The main features that help us to build the DSS System are "FREQ1", "FREQ2", "FREQ3", "FREQ1_D", "FREQ2_D", "FREQ3_D". this features related to the frequency of the drugs's uses. there are several types of drugs and the features that related to the drugs are "SUB1" --> Primary drug, "SUB2" --> Secondary drug, "SUB3" --> Third drug.
in this step we removed all the invalid combinations, if a record have invalid/unknown/none values in all of them together we removed it. 

### The values of frequency columns:

|Values                          |
|------------------------------  |
|1 --> No use in the past month  |
|2 --> Some use                  |
|3 --> Daily use                 |
|-9 --> Missing/Unknown/Invalid  |

### The values of drugs columns:

|Values                             |
|-----------------------------------|
|-9 --> Missing/Unknown/Invalid     |
|1  --> No use in the past month    |
|2  --> Some use                    |
|3  --> Daily use                   |
|4  --> Marijuana/hashish           |
|5  --> Heroin                      |
|6  --> Non-prescription methadone  |
|7  --> Other opiates and synthetics|
|8  --> PCP                         |
|9  --> Hallucinogens               |
|10 --> Methamphetamine/speed       |
|11 --> Other amphetamines          |
|12 --> Other stimulants            |
|13 --> Benzodiazepines             |
|14 --> Other tranquilizers         |
|15 --> Barbiturates                |
|16 --> Other sedatives or hypnotics|
|17 --> Inhalants                   |
|18 --> Over-the-counter medications|
|19 --> Other drugs                 |


## Step 2: Exploratory Data Analysis (EDA)

Before training the prediction model, we did some EDA steps to select the best set of features for the model. we searched after some insights about the data by creating statistical graphs about the features, them frequency, detect null values and etc.

## Step 3: Feature Selection and Engineering

In this step, the code performs feature engineering by adding new features. the features that have been added are built in three phases- 
GRADE1, GRADE2, GRADE3 --> related to the improvment/disimprovment of a patient in the frequencies at admission in compare to the frequencies at dischare. if a patient stay at the same level of frequency use the score is 50, for each increase/decrease of a unit in the level of frequency use the differnce is +/- 25. The range for the grades is 0-100.
GRADE --> this is a Weighted mean based on the grades in GRADE1, GRADE2, GRADE3. the weights for the grades are 1/2 for GRADE1, 1/3 for GRADE2 and 1/6 for GRADE3.
assessment --> this feature is the final feature that represent bins for the grades. We have no interest to predict a specific grade, we want to predict a direction for it.

## assessment values:

|Values                |Range of grades |
|--------------------- |-------------   |
|Very poor improvement |0 - 48          |
|Poor improvement      |48.0001 - 50    |
|some improvement      |50.0001 - 70    |
|Good improvement      |70.0001 - 84    |
|Excellent improvement |84.0001 - 100   |

the code also performs feature selection by computing the correlation matrix for the remaining numeric columns in the dataset. A heatmap is plotted to visualize the correlations between features. The code also identifies pairs of features with a correlation higher than 0.5 and presents them in a table.

## Step 4: Model Training and Evaluation

This step involves splitting the data into training and testing sets. The code randomly selects 70% of the data for training and assigns the remaining 30% for testing. The data is also balanced using under-sampling to address the class imbalance issue in the "Poor improvement" class.
the code trains a prediction model using the prepared data. Various machine learning algorithms, such as multiclass logistic regression and random forests. we did both because to be sure about the relationship between the features and the target. The code includes functions to train the model using the training set, to predict the values of the testing set, and evaluate its performance using appropriate evaluation metrics such as accuracy, precision, recall, or mean squared error. we evaluate the performance of all the models we built.

## Step 5: Decision Support System Implementation

Once the prediction model is trained and evaluated, it is integrated into the Decision Support System. The code provides an interface to input new patient information and simulates multiple treatment scenarios based on the trained model. The predicted improvements for each treatment scenario are calculated (assessement), and the code identifies the set of successful treatments that yield the highest improvement.

## Step 6: Generating Treatment Recommendations

Based on the predictions from the DSS, the code generates treatment recommendations for the new patient. These recommendations consist of a set of successful treatment scenarios that are likely to result in the best improvement. The code provides functions to analyze the predictions, rank the treatments based on predicted improvement, and present the recommendations to the user.

## Step 7: Iterative Improvement and Refinement

The DSS can be continuously improved and refined by incorporating new data and updating the prediction model. As more patient data becomes available, the code allows for iterative training of the model to enhance the accuracy of predictions. This iterative process ensures that the DSS remains up-to-date and aligned with the latest patient outcomes.

__________________________________________________________________________________________________________________


Our data folder contains one data file named tedsd_puf_2020_r.
Each column of the data set is described in detail below according to the following format:

column_name[label, Type, Measurement Level] - Variable Descriptions.
Variable Values = [ ...] 
__________________________________________________________________________________________

DISYR [Year of discharge, numeric, Scale] = Year of client's discharge to substance use 
treatment


Variable Values = [ 2020] 

-----------------------------------

CASEID [Case identification number, numeric, Scale] = Program generated case 
(record)identifier. 


Variable Values = A unique identifier for each case
An example for values in this column:
[1243074, 
1168758,1150846, 1121864, 1180820, 1312442, …]
 

-----------------------------------

STFIPS [Census state FIPS code, numeric, Scale] = State FIPS codes consistent with 
thoseused by the U.S. Census Bureau.  


Variable Values = [1=Alabama, 2=Alaska, 4=Arizona, 5=Arkansas, 6=California, 8=Colorado, 
9=Connecticut,10=Delaware, 
11=District of Columbia, 12=Florida, 13=Georgia,  15=Hawaii, 
16=Idaho,17=Illinois, 18=Indiana, 19=Iowa, 20=Kansas, 21=Kentucky, 22=Louisiana, 
23=Maine,24=Maryland, 25=Massachusetts,
26=Michigan, 27=Minnesota, 28=Mississippi, 
29=Missouri,30=Montana, 31=Nebraska, 32=Nevada,
 33=New Hampshire, 34=New Jersey,35=New 
Mexico,36=New York, 37=North Carolina, 38=North Dakota, 39=Ohio,
40=Oklahoma, 41=Oregon, 
42=Pennsylvania,44=Rhode Island, 45=South Carolina, 46=South Dakota, 47=Tennessee,
48=Texas, 
49=Utah,50=Vermont, 51=Virginia, 53=Washington, 54=West Virginia, 55=Wisconsin, 
56=Wyoming,72=Puerto Rico] 

-----------------------------------

CBSA2010 [CBSA 2010 code, numeric, Scale] = The term 'Core Based Statistical Area' 
(CBSA)is a collective term for both metro and micro areas. Metropolitan and micropolitan 
statisticalareas (metro and micro areas) are geographic entities defined by the U.S. Office of 
Managementand Budget (OMB) for use by federal statistical agencies in collecting, tabulating, 
andpublishing federal statistics. A metro area contains a core urban area with a 
populationof at least 50,000, and a micro area contains an urban core with a population of at least 
10,000but less than 50,000. Each metro or micro area consists of one or more counties and 
includesthe counties containing the core urban area, as well as any adjacent counties that have a 
highdegree of social and economic integration (as measured by commuting to work) with the 
urbancore. 


Variable Values = Example of the values for this variable: 
[-9= Missing/unknown/not 
collected/invalid,10020= Abbeville, LA Micropolitan Statistical Area, 10100= Aberdeen, SD 
MicropolitanStatistical Area, 10140= Aberdeen, WA Micropolitan Statistical Area, …] 

-----------------------------------

EDUC [Education, numeric, Scale] = This field specifies a) the highest school grade 
completedfor adults or children not attending school or b) current school grade for school-age 
children(3-17 years old) attending school.
Guidelines: States that use specific categories 
fordesignating education level should map their categories to a logical number of years of 
schoolcompleted. The mapping should be recorded in the state crosswalk. For example, a state 
categoryof 'associate's degree' would be mapped to 4; 'bachelor's degree' would be mapped to 5, 
etc.


Variable Values = [-9=Missing/unknown/not collected/invalid,
1=Less than one school grade, no 
schooling,nursery school, or kindergarten to Grade 8,
2=Grades 9 to 11, 3=Grade 12 (or GED), 4=1-3 
yearsof college, university, or vocational school, 
5=4 years of college, university, 
BA/BS,some postgraduate study, or more] 

-----------------------------------

MARSTAT [Marital status, numeric, Scale] = This field describes the client's marital 
status.The following categories are compatible with categories used in the U.S. Census. 
• 
Nevermarried: Includes clients who are single or whose only marriage was annulled.
• Now 
married:Includes married couples, those living together as married, living with partners, or 
cohabiting.
• Separated: Includes those legally separated or otherwise absent from spouse 
becauseof marital discord. 
• Divorced, widowed 

 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1=Never married, 2=Now married, 
3=Separated,4=Divorced/widowed] 

-----------------------------------

SERVICES [Type of treatment service/setting at admission, numeric, Nominal] = This 
fielddescribes the type of treatment service or treatment setting in which the client is 
placedat the time of admission or transfer.
• Detoxification, 24-hour service, hospital 
inpatient:
24hours per day medical acute care services in hospital setting for detoxification of 
personswith severe medical complications associated with withdrawal.
• Detoxification, 
24-hourservice, free-standing residential:
24 hours per day services in non-hospital 
settingproviding for safe withdrawal and transition to ongoing treatment.
• 
Rehabilitation/Residential– hospital (other than detoxification):
24 hours per day medical care in a hospital 
facilityin conjunction with treatment services for alcohol and other drug use and 
dependency.
•Rehabilitation/Residential – short term (30 days or fewer):
Typically, 30 days or 
fewerof non-acute care in a setting with treatment services for alcohol and other drug use and 
dependency.
•Rehabilitation/Residential – long term (more than 30 days):
Typically, more than 30 
daysof non-acute care in a setting with treatment services for alcohol and other drug use and 
dependency;may include transitional living arrangements such as halfway houses.
• Ambulatory - 
intensiveoutpatient:
At a minimum, treatment lasting two or more hours per day for 3 or more days 
perweek.
• Ambulatory - non-intensive outpatient:
Ambulatory treatment services 
includingindividual, family and/or group services; may include pharmacological therapies.
• 
Ambulatory- detoxification:
Outpatient treatment services providing for safe withdrawal in an 
ambulatorysetting (pharmacological or non-pharmacological). 


Variable Values = [1=Detox-24-hour-hospital inpatient, 2=Detox-24-hour-free-standing 
residential,
3=Rehab/residential-hospital(non-detox), 4=Rehab/residential-short term (30 days or fewer), 
5=Rehab/residential-longterm (more than 30 days), 6=Ambulatory-intensive outpatient, 
7=Ambulatory-non-intensiveoutpatient, 8=Ambulatory-detoxification]
 

-----------------------------------

DETCRIM [Detailed criminal justice referral, numeric, Scale] = This field provides 
moredetailed information about those clients who are coded as '07 Criminal justice 
referral'in Referral Source.
• State/federal court
• Other court – Court other than state or 
federalcourt
• Probation/parole
• Other recognized legal entity: For example, local law 
enforcementagency, corrections agency, youth services, review board/agency.
• Diversionary 
program– For example, TASC
• Prison
• DUI/DWI
• Other
Guidelines: This field is to be used 
onlyif principal source of referral in the Minimum Data Set field is coded 07, 'criminal 
justicereferral.' For all other principal source of referral codes (01 to 06 and missing), this 
fieldshould be coded as missing 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1=State/federal court, 2=Formal 
adjudicationprocess, 3=Probation/parole, 4=Other recognized legal entity, 5=Diversionary 
program,
6=Prison,7=DUI/DWI, 8=Other] 

-----------------------------------

LOS [Length of stay in treatment (days), numeric, Scale] = Number of days the patient 
stayedin treatment.  


Variable Values = [1=1, 2=2, 3=3, 4=4, 5=5, 6=6, 7=7, 8=8, 9=9, 10=10, 11=11, 12=12, 13=13, 14=14, 15=15, 
16=16,17=17, 18=18, 19=19, 20=20, 21=21, 22=22, 23=23, 24=24, 25=25, 26=26, 27=27, 28=28, 
29=29,30=30, 31=31 to 45 days, 32=46 to 60 days, 33=61 to 90 days, 34=91 to 120 days, 35=121 to 180 
days,36=181 to 365 days, 37=More than a year]
 

-----------------------------------

PSOURCE [Referral source, numeric, Scale] = This field describes the person or agency 
referringthe client to treatment:
• Individual (includes self-referral): Includes the 
client,a family member, friend, or any other individual who would not be included in any of the 
followingcategories; includes self-referral due to pending DWI/DUI.
• Alcohol/drug use care 
provider:Any program, clinic, or other health care provider whose principal objective is 
treatingclients with substance use diagnosis, or a program whose activities are related to 
alcoholor other drug use prevention, education, or treatment.
• Other health care provider: A 
physician,psychiatrist, or other licensed health care professional; or general hospital, 
psychiatrichospital, mental health program, or nursing home.
• School (educational): A school 
principal,counselor, or teacher; or a student assistance program (SAP), the school system, or an 
educationalagency.
• Employer/EAP: A supervisor or an employee counselor.
• Other community 
referral:Community or religious organization or any federal, state, or local agency that 
providesaid in the areas of poverty relief, unemployment, shelter, or social welfare. This 
categoryalso includes defense attorneys and self-help groups such as Alcoholics Anonymous 
(AA),Al-Anon, and Narcotics Anonymous (NA).
• Court/criminal justice referral/DUI/DWI: 
Anypolice official, judge, prosecutor, probation officer or other person affiliated 
witha federal, state, or county judicial system. Includes referral by a court for DWI/DUI, 
clientsreferred in lieu of or for deferred prosecution, or during pre-trial release, or before 
orafter official adjudication. Includes clients on pre-parole, pre-release, work or 
homefurlough or TASC. Client need not be officially designated as “on parole.” Includes 
clientsreferred through civil commitment. Clients in this category are further defined in 
DetailedCriminal Justice Referral 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1=Individual (includes 
self-referral),2=Alcohol/drug use care provider, 3=Other health care provider, 4=School 
(educational),5=Employer/EAP, 6=Other community referral, 7=Court/criminal justice 
referral/DUI/DWI]


-----------------------------------

NOPRIOR [Previous substance use treatment episodes, numeric, Scale] = Indicates the 
numberof previous treatment episodes the client has received in any substance use treatment 
program.Changes in service for the same episode (transfers) should not be counted as separate 
priorepisodes.
Guidelines: This field measures the substance use treatment history of the 
clientonly. This does not include or pertain to the client's mental health treatment history. 
Itis preferred that the number of prior treatments be a self-reported field collected at 
thetime of client intake. However, this data field may be derived from the state data 
system,if the system has that capability, and episodes can be counted for at least several 
years.


Variable Values = [-9=Missing/unknown/not collected/invalid, 0=No prior treatment episode,
1=One 
ormore prior treatment episodes]
 

-----------------------------------

ARRESTS [Arrests in past 30 days prior to admission, numeric, Scale] = Indicates the 
numberof arrests in the 30 days prior to the reference date (i.e., date of admission or date of 
discharge).This field is intended to capture the number of times the client was arrested (not the 
numberof charges) for any cause during the reference period. Any formal arrest should be 
counted,regardless of whether incarceration or conviction resulted.
Guidelines: This field 
isintended to capture the number of times the client was arrested for any cause during the 
30days preceding the date of admission to treatment. Any formal arrest is to be counted 
regardlessof whether incarceration or conviction resulted and regardless of the status of 
proceedingsincident to the arrest at the time of admission. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 0= None, 1= Once, 2= Two or more times]
 

-----------------------------------

EMPLOY [Employment status at admission, numeric, Scale] = This field identifies the 
client’semployment status.
• Full-time: Working 35 hours or more each week, including active 
dutymembers of the uniformed services.
• Part-time: Working fewer than 35 hours each 
week.
•Unemployed: Looking for work during the past 30 days or on layoff from a job.
• Not in 
laborforce: Not looking for work during the past 30 days or a student, homemaker, disabled, 
retired,or an inmate of an institution. Clients in this category are further defined in Detailed 
Notin Labor Force.
Guidelines: Seasonal workers are coded in this category based on their 
employmentstatus at the time of admission. For example, if they are employed full time at the time of 
admission,they are coded as 01. If they are not in the labor force at the time of admission, they are 
coded04. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Full-time, 2= Part-time, 3= 
Unemployed,4= Not in labor force]
 

-----------------------------------

METHUSE [Medication-assisted opioid therapy, numeric, Scale] = This field 
identifieswhether the use of opioid medications such as methadone, buprenorphine, and/or 
naltrexoneis part of the client’s treatment plan. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Yes, 2= No]
 

-----------------------------------

PSYPROB [Co-occurring mental and substance use disorders, numeric, Scale] = 
Indicateswhether the client has co-occurring mental and substance use disorders 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Yes, 2= No] 

-----------------------------------

PREG [Pregnant at admission, numeric, Scale] = This field indicates whether a female 
clientwas pregnant at the time of admission.
Guidelines: All male clients were recoded to 
missingfor this variable due to the item being not applicable. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Yes, 2= No] 

-----------------------------------

GENDER [Gender, numeric, Scale] = This field identifies the client's biological sex 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Male, 2= Female] 

-----------------------------------

VET [Veteran status, numeric, Scale] = This field indicates whether the client has 
servedin the uniformed services (Army, Navy, Air Force, Marine Corps, Coast Guard, Public 
HealthService Commissioned Corps, Coast and Geodetic Survey, etc.).
Guidelines: A veteran 
isa person 16 years or older who has served (even for a short time), but is not currently 
serving,on active duty in the U.S. Army, Navy, Marine Corps, Coast Guard, or Commissioned Corps 
ofthe U.S. Public Health Service or National Oceanic and Atmospheric Administration, or 
whoserved as a Merchant Marine seaman during World War II. Persons who served in the 
NationalGuard or Military Reserves are classified as veterans only if they were ever called or 
orderedto active duty, not counting the 4–6 months for initial training or yearly summer camps. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Yes, 2= No] 

-----------------------------------

LIVARAG [Living arrangements at admission, numeric, Scale] = Identifies whether the 
clientis homeless, a dependent (living with parents or in a supervised setting) or living 
independentlyon his or her own.
• Homeless: Clients with no fixed address; includes homeless 
shelters.
•Dependent living: Clients living in a supervised setting such as a residential 
institution,halfway house, or group home, and children (under age 18) living with parents, 
relatives,or guardians or (substance use clients only) in foster care.
• Independent living: 
Clientsliving alone or with others in a private residence and capable of self-care. Includes 
adultchildren (age 18 and over) living with parents and adolescents living independently. 
Also,includes clients who live independently with case management or supported housing 
support.


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Homeless, 2= Dependent living,
3= 
Independentliving] 

-----------------------------------

DAYWAIT [Days waiting to enter substance use treatment, numeric, Scale] = Indicates 
thenumber of days from the first contact or request for a substance use treatment service 
untilthe client was admitted and the first clinical substance use treatment service was 
provided.
Guidelines:This field is intended to capture the number of days the client must wait to begin 
treatmentbecause of program capacity, treatment availability, admissions requirements, or 
otherprogram requirements. It should not include time delays caused by client 
unavailabilityor client failure to meet any requirement or obligation. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 0= 0, 1= 1-7, 2= 8-14, 3= 15-30,
4= 31 or 
more]


-----------------------------------

SERVICES_D [Type of treatment service/setting at discharge, numeric, Nominal] = This 
fielddescribes the type of treatment service or treatment setting in which the client is 
placedat the time of discharge.
• Detoxification, 24-hour service, hospital inpatient:
24 
hoursper day medical acute care services in hospital setting for detoxification of persons 
withsevere medical complications associated with withdrawal.
• Detoxification, 
24-hourservice, free-standing residential:
24 hours per day services in non-hospital 
settingproviding for safe withdrawal and transition to ongoing treatment.
• 
Rehabilitation/Residential– hospital (other than detoxification):
24 hours per day medical care in a hospital 
facilityin conjunction with treatment services for alcohol and other drug use and 
dependency.
•Rehabilitation/Residential – short term (30 days or fewer):
Typically, 30 days or 
fewerof non-acute care in a setting with treatment services for alcohol and other drug use and 
dependency.
•Rehabilitation/Residential – long term (more than 30 days):
Typically, more than 30 
daysof non-acute care in a setting with treatment services for alcohol and other drug use and 
dependency;may include transitional living arrangements such as halfway houses.
• Ambulatory - 
intensiveoutpatient:
At a minimum, treatment lasting two or more hours per day for 3 or more days 
perweek.
• Ambulatory - non-intensive outpatient:
Ambulatory treatment services 
includingindividual, family and/or group services; may include pharmacological therapies.
• 
Ambulatory- detoxification:
Outpatient treatment services providing for safe withdrawal in an 
ambulatorysetting (pharmacological or non-pharmacological). 


Variable Values = [1=Detox-24-hour-hospital inpatient, 2=Detox-24-hour-free-standing 
residential,
3=Rehab/residential-hospital(non-detox), 4=Rehab/residential-short term (30 days or fewer), 
5=Rehab/residential-longterm (more than 30 days), 6=Ambulatory-intensive outpatient, 
7=Ambulatory-non-intensiveoutpatient, 8=Ambulatory-detoxification] 

-----------------------------------

REASON [Reason for discharge, numeric, Nominal] = The reason of the discharge from the 
treatment. 


Variable Values = [1= Treatment completed, 2= Dropped out of treatment, 3=Terminated by facility, 
4= 
Transferredto another treatment program or facility, 5= Incarcerated, 6= Death, 
7= Other]
 

-----------------------------------

EMPLOY_D [Employment status at discharge, numeric, Scale] = This field identifies the 
client’semployment status.
• Full-time: Working 35 hours or more each week, including active 
dutymembers of the uniformed services.
• Part-time: Working fewer than 35 hours each 
week.
•Unemployed: Looking for work during the past 30 days or on layoff from a job.
• Not in 
laborforce: Not looking for work during the past 30 days or a student, homemaker, disabled, 
retired,or an inmate of an institution. Clients in this category are further defined in Detailed 
Notin Labor Force.
Guidelines: Seasonal workers are coded in this category based on their 
employmentstatus at the time of discharge. For example, if they are employed full time at the time of 
discharge,they are coded as 01. If they are not in the labor force at the time of discharge, they are 
coded04. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Full-time, 2= Part-time, 3= 
Unemployed,4= Not in labor force] 

-----------------------------------

LIVARAG_D [Living arrangements at discharge, numeric, Scale] = Identifies whether 
theclient is homeless, a dependent (living with parents or in a supervised setting) or 
livingindependently on his or her own.
• Homeless: Clients with no fixed address; includes 
homelessshelters.
• Dependent living: Clients living in a supervised setting such as a 
residentialinstitution, halfway house, or group home, and children (under age 18) living with 
parents,relatives, or guardians or (substance use clients only) in foster care.
• Independent 
living:Clients living alone or with others in a private residence and capable of self-care. 
Includesadult children (age 18 and over) living with parents and adolescents living 
independently.Also, includes clients who live independently with case management or supported 
housingsupport. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 1= Homeless, 2= Dependent living,
3= 
Independentliving] 

-----------------------------------

ARRESTS_D [Arrests in past 30 days prior to discharge, numeric, Scale] = Indicates the 
numberof arrests in the 30 days prior to the reference date (i.e., date of admission or date of 
discharge).This field is intended to capture the number of times the client was arrested (not the 
numberof charges) for any cause during the reference period. Any formal arrest should be 
counted,regardless of whether incarceration or conviction resulted.
Guidelines: This field 
isintended to capture the number of times the client was arrested for any cause during the 
30days preceding the date of admission to treatment. Any formal arrest is to be counted 
regardlessof whether incarceration or conviction resulted and regardless of the status of 
proceedingsincident to the arrest at the time of admission. 


Variable Values = [-9=Missing/unknown/not collected/invalid, 0= None, 1= Once, 2= Two or more times] 

-----------------------------------

DSMCRIT [DSM diagnosis (SuDS 4 or SuDS 19), numeric, Scale] = Client's diagnosis is used 
toidentify the substance use problem that provides the reason for client encounter or 
treatment.This can be reported by using either the Diagnostic and Statistical Manual of Mental 
Disorders(DSM) from the American Psychiatric Association or the International Classification 
ofDiseases (ICD), from the World Health Organization.
The discrete diagnosis codes 
havebeen recoded into categories related to use of and dependence on specific substances, 
mentalhealth conditions, and other conditions. Diagnoses reported by states using either 
standardclassification of mental disorders have been combined. 


Variable Values = [-9= Missing/unknown/not collected/invalid/no or deferred diagnosis, 1= 
Alcohol-induceddisorder, 2= Substance-induced disorder, 3= Alcohol intoxication, 
4= Alcohol 
dependence,5= Opioid dependence, 6= Cocaine dependence, 
7= Cannabis dependence, 8= Other 
substancedependence, 9= Alcohol abuse, 
10= Cannabis abuse, 11= Other substance abuse, 12= 
Opioidabuse, 13= Cocaine abuse, 14= Anxiety disorders, 15= Depressive disorders, 16= 
Schizophrenia/otherpsychotic disorders, 17= Bipolar disorders, 18= Attention deficit/disruptive 
behaviordisorders, 19= Other mental health condition]



 

-----------------------------------

AGE [Age at admission, numeric, Nominal] = Calculated from date of birth and date of 
admissionand categorized.  


Variable Values = [1= 12-14 years old, 2= 15-17 years old, 3=18-20 years old, 4= 21-24 years old, 
5= 25-29 
yearsold, 6= 30-34 years old, 7= 35-39 years old, 8= 40-44 years old, 
9= 45-49 years old, 10= 
50-54years old, 11= 55-64 years old, 12= 65 years and older]
 

-----------------------------------

RACE [Race, numeric, Scale] = This field identifies the client's race:
• Alaska Native 
(Aleut,Eskimo): A person having origins in any of the original people of Alaska. This category 
maybe reported if available.
• American Indian or Alaska Native: A person having origins 
inany of the original people of North America and South America (including Central 
Americaand the original peoples of Alaska) and who maintains tribal affiliation or community 
attachment.States collecting Alaska Native should use this category for all other American 
Indians.
•Asian or Pacific Islander: A person having origins in any of the original people of the 
FarEast, the Indian subcontinent, Southeast Asia, or the Pacific Islands. This category 
maybe used only if a state does not collect Asian and Native Hawaiian or Other Pacific 
Islanderseparately.
• Black or African American: A person having origins in any of the black 
racialgroups of Africa.
• White: A person having origins in any of the original people of 
Europe,the Middle East, or North Africa.
• Asian: A person having origins in any of the original 
peopleof the Far East, Southeast Asia, or the Indian subcontinent, including, for example, 
Cambodia,China, India, Japan, Korea, Malaysia, Pakistan, the Philippine Islands, Thailand, 
andVietnam.
• Other single race: Use this category for instances in which the client is not 
identifiedin any category above or whose origin group, because of area custom, is regarded as a 
racialclass distinct from the above categories.
• Two or more races: Use this code when the 
statedata system allows multiple race selection and more than one race is indicated.
• 
NativeHawaiian or Other Pacific Islander: A person having origins in any of the original 
peoplesof Hawaii, Guam, Samoa, or other Pacific Islands.
Guidelines: If the state does not 
distinguishbetween American Indian and Alaska Native, code both as 2, American Indian. States that 
canseparate 'Asian' and 'Native Hawaiian or Other Pacific Islander' should use codes 6 and 
9for those categories. States that cannot make the separation should use the combined 
code3 until the separation becomes possible. Once a state begins using codes 6 and 9, code 3 
shouldno longer be used by that state. States are asked to convert to the new categories when 
possible.


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Alaska Native (Aleut, Eskimo, 
Indian),
2= American Indian (other than Alaska Native), 3= Asian or Pacific Islander, 
4= Black 
orAfrican American, 5= White, 6= Asian, 
7= Other single race, 8= Two or more races, 9= 
NativeHawaiian or Other Pacific Islander] 

-----------------------------------

ETHNIC [Ethnicity, numeric, Scale] = This field identifies client's specific 
Hispanicor Latino origin, if applicable.
• Puerto Rican: Of Puerto Rican origin regardless of 
race.
•Mexican: Of Mexican origin regardless of race.
• Cuban: Of Cuban origin regardless of 
race.
•Other specific Hispanic or Latino: Of known Central or South American or any other 
Spanishculture or origin (including Spain), other than Puerto Rican, Mexican, or Cuban, 
regardlessof race.
• Not of Hispanic or Latino origin
• Hispanic, specific origin not specified: 
OfHispanic or Latino origin, but origin not known or not specified.
Guidelines: If a 
statedoes not collect specific Hispanic detail, this field is coded as 5 - Hispanic or Latino, 
specificorigin not specified. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Puerto Rican, 
2= Mexican, 3= Cuban 
orother specific Hispanic, 4= Not of Hispanic or Latino origin, 
5= Hispanic or 
Latino-specificorigin not specified] 

-----------------------------------

DETNLF [Detailed not in labor force category at admission, numeric, Scale] = 
Provides 
moredetailed information at time of admission about those clients who are coded as '04 Not in 
laborforce' in Employment Status.
Resident of institution: Persons receiving services 
frominstitutional facilities such as hospitals, jails, prisons, long-term residential 
care,etc. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Homemaker, 
2= Student, 3= 
Retired-disabled,4= Resident of institution, 5= Other]

 

-----------------------------------

DETNLF_D [Detailed not in labor force category at discharge, numeric, Scale] = 
Providesmore detailed information at time of discharge about those clients who are coded as '04 
Notin labor force' in Employment Status.
Resident of institution: Persons receiving 
servicesfrom institutional facilities such as hospitals, jails, prisons, long-term 
residentialcare, etc. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Homemaker, 
2= Student, 3= 
Retired-disabled,4= Resident of institution, 5= Other] 

-----------------------------------

PRIMINC [Source of income/support, numeric, Scale] = This field identifies the 
client’sprincipal source of financial support. For children younger than 18 years old, report 
theprimary parental source of income/support. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Wages/salary, 
2= Public 
assistance,3= Retirement/pension, disability, 4= Other, 5= None]
 

-----------------------------------

SUB1 [Substance use at admission (primary), numeric, Scale] = This field identifies 
theclient's primary substance use.
(1) None
(2) Alcohol
(3) Cocaine/crack
(4) 
Marijuana/hashish:Includes THC and any other cannabis sativa preparations.
(5) Heroin
(6) 
Non-prescriptionmethadone
(7) Other opiates and synthetics: Includes buprenorphine, butorphanol, 
codeine,hydrocodone, hydromorphone, meperidine, morphine, opium, oxycodone, pentazocine, 
propoxyphene,tramadol, and other narcotic analgesics, opiates, or synthetics. (8) PCP: 
Phencyclidine
(9)Hallucinogens: Includes LSD, DMT, mescaline, peyote, psilocybin, STP, and other 
hallucinogens.
(10)Methamphetamine/speed
(11) Other amphetamines: Includes amphetamines, MDMA, 
‘bathsalts’, phenmetrazine, and other amines and related drugs. (12) Other stimulants: 
Includesmethylphenidate and any other stimulants.
(13) Benzodiazepines: Includes 
alprazolam,chlordiazepoxide, clonazepam, clorazepate, diazepam, flunitrazepam, flurazepam, 
halazepam,lorazepam, oxazepam, prazepam, temazepam, triazolam, and other unspecified 
benzodiazepines.(14) Other tranquilizers: Includes meprobamate, and other non-benzodiazepine 
tranquilizers.
(15)Barbiturates: Includes amobarbital, pentobarbital, phenobarbital, secobarbital, 
etc.
(16)Other sedatives or hypnotics: Includes chloral hydrate, ethchlorvynol, 
glutethimide,methaqualone, and other non-barbiturate sedatives and hypnotics.
(17) Inhalants: 
Includesaerosols; chloroform, ether, nitrous oxide and other anesthetics; gasoline; glue; 
nitrites;paint thinner and other solvents; and other inappropriately inhaled products.
(18) 
Over-the-countermedications: Includes aspirin, dextromethorphan and other cough syrups, 
diphenhydramineand other anti-histamines, ephedrine, sleep aids, and any other legally obtained, 
non-prescriptionmedication.
(19) Other drugs: Includes diphenylhydantoin/phenytoin, GHB/GBL, 
ketamine,synthetic cannabinoid 'Spice', carisoprodol (Soma), and other drugs. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= None, 2= Alcohol, 3= Cocaine/crack, 

4=Marijuana/hashish, 5= Heroin, 6= Non-prescription methadone, 
7= Other opiates and 
synthetics,8= PCP, 9= Hallucinogens, 
10= Methamphetamine/speed, 11= Other amphetamines, 12= 
Otherstimulants, 
13= Benzodiazepines, 14= Other tranquilizers, 15= Barbiturates, 
16= 
Othersedatives or hypnotics, 17= Inhalants, 18= Over-the-counter medications, 19= Other 
drugs]



-----------------------------------

SUB2 [Substance use at admission (secondary), numeric, Scale] = This field identifies 
theclient's secondary substance use.
(1) None
(2) Alcohol
(3) Cocaine/crack
(4) 
Marijuana/hashish:Includes THC and any other cannabis sativa preparations.
(5) Heroin
(6) 
Non-prescriptionmethadone
(7) Other opiates and synthetics: Includes buprenorphine, butorphanol, 
codeine,hydrocodone, hydromorphone, meperidine, morphine, opium, oxycodone, pentazocine, 
propoxyphene,tramadol, and other narcotic analgesics, opiates, or synthetics. (8) PCP: 
Phencyclidine
(9)Hallucinogens: Includes LSD, DMT, mescaline, peyote, psilocybin, STP, and other 
hallucinogens.
(10)Methamphetamine/speed
(11) Other amphetamines: Includes amphetamines, MDMA, 
‘bathsalts’, phenmetrazine, and other amines and related drugs. (12) Other stimulants: 
Includesmethylphenidate and any other stimulants.
(13) Benzodiazepines: Includes 
alprazolam,chlordiazepoxide, clonazepam, clorazepate, diazepam, flunitrazepam, flurazepam, 
halazepam,lorazepam, oxazepam, prazepam, temazepam, triazolam, and other unspecified 
benzodiazepines.(14) Other tranquilizers: Includes meprobamate, and other non-benzodiazepine 
tranquilizers.
(15)Barbiturates: Includes amobarbital, pentobarbital, phenobarbital, secobarbital, 
etc.
(16)Other sedatives or hypnotics: Includes chloral hydrate, ethchlorvynol, 
glutethimide,methaqualone, and other non-barbiturate sedatives and hypnotics.
(17) Inhalants: 
Includesaerosols; chloroform, ether, nitrous oxide and other anesthetics; gasoline; glue; 
nitrites;paint thinner and other solvents; and other inappropriately inhaled products.
(18) 
Over-the-countermedications: Includes aspirin, dextromethorphan and other cough syrups, 
diphenhydramineand other anti-histamines, ephedrine, sleep aids, and any other legally obtained, 
non-prescriptionmedication.
(19) Other drugs: Includes diphenylhydantoin/phenytoin, GHB/GBL, 
ketamine,synthetic cannabinoid 'Spice', carisoprodol (Soma), and other drugs. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= None, 2= Alcohol, 3= Cocaine/crack, 

4=Marijuana/hashish, 5= Heroin, 6= Non-prescription methadone, 
7= Other opiates and 
synthetics,8= PCP, 9= Hallucinogens, 
10= Methamphetamine/speed, 11= Other amphetamines, 12= 
Otherstimulants, 
13= Benzodiazepines, 14= Other tranquilizers, 15= Barbiturates, 
16= 
Othersedatives or hypnotics, 17= Inhalants, 18= Over-the-counter medications, 19= Other 
drugs]


-----------------------------------

SUB3 [Substance use at admission (tertiary), numeric, Scale] = This field identifies 
theclient's tertiary substance use.
(1) None
(2) Alcohol
(3) Cocaine/crack
(4) 
Marijuana/hashish:Includes THC and any other cannabis sativa preparations.
(5) Heroin
(6) 
Non-prescriptionmethadone
(7) Other opiates and synthetics: Includes buprenorphine, butorphanol, 
codeine,hydrocodone, hydromorphone, meperidine, morphine, opium, oxycodone, pentazocine, 
propoxyphene,tramadol, and other narcotic analgesics, opiates, or synthetics. (8) PCP: 
Phencyclidine
(9)Hallucinogens: Includes LSD, DMT, mescaline, peyote, psilocybin, STP, and other 
hallucinogens.
(10)Methamphetamine/speed
(11) Other amphetamines: Includes amphetamines, MDMA, 
‘bathsalts’, phenmetrazine, and other amines and related drugs. (12) Other stimulants: 
Includesmethylphenidate and any other stimulants.
(13) Benzodiazepines: Includes 
alprazolam,chlordiazepoxide, clonazepam, clorazepate, diazepam, flunitrazepam, flurazepam, 
halazepam,lorazepam, oxazepam, prazepam, temazepam, triazolam, and other unspecified 
benzodiazepines.(14) Other tranquilizers: Includes meprobamate, and other non-benzodiazepine 
tranquilizers.
(15)Barbiturates: Includes amobarbital, pentobarbital, phenobarbital, secobarbital, 
etc.
(16)Other sedatives or hypnotics: Includes chloral hydrate, ethchlorvynol, 
glutethimide,methaqualone, and other non-barbiturate sedatives and hypnotics.
(17) Inhalants: 
Includesaerosols; chloroform, ether, nitrous oxide and other anesthetics; gasoline; glue; 
nitrites;paint thinner and other solvents; and other inappropriately inhaled products.
(18) 
Over-the-countermedications: Includes aspirin, dextromethorphan and other cough syrups, 
diphenhydramineand other anti-histamines, ephedrine, sleep aids, and any other legally obtained, 
non-prescriptionmedication.
(19) Other drugs: Includes diphenylhydantoin/phenytoin, GHB/GBL, 
ketamine,synthetic cannabinoid 'Spice', carisoprodol (Soma), and other drugs. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= None, 2= Alcohol, 3= Cocaine/crack, 

4=Marijuana/hashish, 5= Heroin, 6= Non-prescription methadone, 
7= Other opiates and 
synthetics,8= PCP, 9= Hallucinogens, 
10= Methamphetamine/speed, 11= Other amphetamines, 12= 
Otherstimulants, 
13= Benzodiazepines, 14= Other tranquilizers, 15= Barbiturates, 
16= 
Othersedatives or hypnotics, 17= Inhalants, 18= Over-the-counter medications, 19= Other 
drugs]


-----------------------------------

SUB1_D [Substance use at discharge (primary), numeric, Scale] = This field identifies 
theclient's tertiary substance use.
(1) None
(2) Alcohol
(3) Cocaine/crack
(4) 
Marijuana/hashish:Includes THC and any other cannabis sativa preparations.
(5) Heroin
(6) 
Non-prescriptionmethadone
(7) Other opiates and synthetics: Includes buprenorphine, butorphanol, 
codeine,hydrocodone, hydromorphone, meperidine, morphine, opium, oxycodone, pentazocine, 
propoxyphene,tramadol, and other narcotic analgesics, opiates, or synthetics. (8) PCP: 
Phencyclidine
(9)Hallucinogens: Includes LSD, DMT, mescaline, peyote, psilocybin, STP, and other 
hallucinogens.
(10)Methamphetamine/speed
(11) Other amphetamines: Includes amphetamines, MDMA, 
‘bathsalts’, phenmetrazine, and other amines and related drugs. (12) Other stimulants: 
Includesmethylphenidate and any other stimulants.
(13) Benzodiazepines: Includes 
alprazolam,chlordiazepoxide, clonazepam, clorazepate, diazepam, flunitrazepam, flurazepam, 
halazepam,lorazepam, oxazepam, prazepam, temazepam, triazolam, and other unspecified 
benzodiazepines.(14) Other tranquilizers: Includes meprobamate, and other non-benzodiazepine 
tranquilizers.
(15)Barbiturates: Includes amobarbital, pentobarbital, phenobarbital, secobarbital, 
etc.
(16)Other sedatives or hypnotics: Includes chloral hydrate, ethchlorvynol, 
glutethimide,methaqualone, and other non-barbiturate sedatives and hypnotics.
(17) Inhalants: 
Includesaerosols; chloroform, ether, nitrous oxide and other anesthetics; gasoline; glue; 
nitrites;paint thinner and other solvents; and other inappropriately inhaled products.
(18) 
Over-the-countermedications: Includes aspirin, dextromethorphan and other cough syrups, 
diphenhydramineand other anti-histamines, ephedrine, sleep aids, and any other legally obtained, 
non-prescriptionmedication.
(19) Other drugs: Includes diphenylhydantoin/phenytoin, GHB/GBL, 
ketamine,synthetic cannabinoid 'Spice', carisoprodol (Soma), and other drugs. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= None, 2= Alcohol, 3= Cocaine/crack, 

4=Marijuana/hashish, 5= Heroin, 6= Non-prescription methadone, 
7= Other opiates and 
synthetics,8= PCP, 9= Hallucinogens, 
10= Methamphetamine/speed, 11= Other amphetamines, 12= 
Otherstimulants, 
13= Benzodiazepines, 14= Other tranquilizers, 15= Barbiturates, 
16= 
Othersedatives or hypnotics, 17= Inhalants, 18= Over-the-counter medications, 19= Other 
drugs]


-----------------------------------

SUB2_D [Substance use at discharge (secondary), numeric, Scale] = This field 
identifiesthe client's tertiary substance use.
(1) None
(2) Alcohol
(3) Cocaine/crack
(4) 
Marijuana/hashish:Includes THC and any other cannabis sativa preparations.
(5) Heroin
(6) 
Non-prescriptionmethadone
(7) Other opiates and synthetics: Includes buprenorphine, butorphanol, 
codeine,hydrocodone, hydromorphone, meperidine, morphine, opium, oxycodone, pentazocine, 
propoxyphene,tramadol, and other narcotic analgesics, opiates, or synthetics. (8) PCP: 
Phencyclidine
(9)Hallucinogens: Includes LSD, DMT, mescaline, peyote, psilocybin, STP, and other 
hallucinogens.
(10)Methamphetamine/speed
(11) Other amphetamines: Includes amphetamines, MDMA, 
‘bathsalts’, phenmetrazine, and other amines and related drugs. (12) Other stimulants: 
Includesmethylphenidate and any other stimulants.
(13) Benzodiazepines: Includes 
alprazolam,chlordiazepoxide, clonazepam, clorazepate, diazepam, flunitrazepam, flurazepam, 
halazepam,lorazepam, oxazepam, prazepam, temazepam, triazolam, and other unspecified 
benzodiazepines.(14) Other tranquilizers: Includes meprobamate, and other non-benzodiazepine 
tranquilizers.
(15)Barbiturates: Includes amobarbital, pentobarbital, phenobarbital, secobarbital, 
etc.
(16)Other sedatives or hypnotics: Includes chloral hydrate, ethchlorvynol, 
glutethimide,methaqualone, and other non-barbiturate sedatives and hypnotics.
(17) Inhalants: 
Includesaerosols; chloroform, ether, nitrous oxide and other anesthetics; gasoline; glue; 
nitrites;paint thinner and other solvents; and other inappropriately inhaled products.
(18) 
Over-the-countermedications: Includes aspirin, dextromethorphan and other cough syrups, 
diphenhydramineand other anti-histamines, ephedrine, sleep aids, and any other legally obtained, 
non-prescriptionmedication.
(19) Other drugs: Includes diphenylhydantoin/phenytoin, GHB/GBL, 
ketamine,synthetic cannabinoid 'Spice', carisoprodol (Soma), and other drugs. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= None, 2= Alcohol, 3= Cocaine/crack, 

4=Marijuana/hashish, 5= Heroin, 6= Non-prescription methadone, 
7= Other opiates and 
synthetics,8= PCP, 9= Hallucinogens, 
10= Methamphetamine/speed, 11= Other amphetamines, 12= 
Otherstimulants, 
13= Benzodiazepines, 14= Other tranquilizers, 15= Barbiturates, 
16= 
Othersedatives or hypnotics, 17= Inhalants, 18= Over-the-counter medications, 19= Other 
drugs]


-----------------------------------

SUB3_D [Substance use at discharge (tertiary), numeric, Scale] = This field 
identifiesthe client's tertiary substance use.
(1) None
(2) Alcohol
(3) Cocaine/crack
(4) 
Marijuana/hashish:Includes THC and any other cannabis sativa preparations.
(5) Heroin
(6) 
Non-prescriptionmethadone
(7) Other opiates and synthetics: Includes buprenorphine, butorphanol, 
codeine,hydrocodone, hydromorphone, meperidine, morphine, opium, oxycodone, pentazocine, 
propoxyphene,tramadol, and other narcotic analgesics, opiates, or synthetics. (8) PCP: 
Phencyclidine
(9)Hallucinogens: Includes LSD, DMT, mescaline, peyote, psilocybin, STP, and other 
hallucinogens.
(10)Methamphetamine/speed
(11) Other amphetamines: Includes amphetamines, MDMA, 
‘bathsalts’, phenmetrazine, and other amines and related drugs. (12) Other stimulants: 
Includesmethylphenidate and any other stimulants.
(13) Benzodiazepines: Includes 
alprazolam,chlordiazepoxide, clonazepam, clorazepate, diazepam, flunitrazepam, flurazepam, 
halazepam,lorazepam, oxazepam, prazepam, temazepam, triazolam, and other unspecified 
benzodiazepines.(14) Other tranquilizers: Includes meprobamate, and other non-benzodiazepine 
tranquilizers.
(15)Barbiturates: Includes amobarbital, pentobarbital, phenobarbital, secobarbital, 
etc.
(16)Other sedatives or hypnotics: Includes chloral hydrate, ethchlorvynol, 
glutethimide,methaqualone, and other non-barbiturate sedatives and hypnotics.
(17) Inhalants: 
Includesaerosols; chloroform, ether, nitrous oxide and other anesthetics; gasoline; glue; 
nitrites;paint thinner and other solvents; and other inappropriately inhaled products.
(18) 
Over-the-countermedications: Includes aspirin, dextromethorphan and other cough syrups, 
diphenhydramineand other anti-histamines, ephedrine, sleep aids, and any other legally obtained, 
non-prescriptionmedication.
(19) Other drugs: Includes diphenylhydantoin/phenytoin, GHB/GBL, 
ketamine,synthetic cannabinoid 'Spice', carisoprodol (Soma), and other drugs. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= None, 2= Alcohol, 3= Cocaine/crack, 

4=Marijuana/hashish, 5= Heroin, 6= Non-prescription methadone, 
7= Other opiates and 
synthetics,8= PCP, 9= Hallucinogens, 
10= Methamphetamine/speed, 11= Other amphetamines, 12= 
Otherstimulants, 
13= Benzodiazepines, 14= Other tranquilizers, 15= Barbiturates, 
16= 
Othersedatives or hypnotics, 17= Inhalants, 18= Over-the-counter medications, 19= Other 
drugs]


-----------------------------------

ROUTE1 [Route of administration (primary), numeric, Scale] = This field identifies 
theusual route of administration of the corresponding substance identified in Substance 
Use(SUB1). 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Oral, 
2= Smoking, 3= Inhalation, 
disabled,4= Injection (intravenous, intramuscular, intradermal, or subcutaneous), 5= 
Other]


-----------------------------------

ROUTE2 [Route of administration (secondary), numeric, Scale] = This field identifies 
theusual route of administration of the corresponding substance identified in Substance 
Use(SUB2). 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Oral, 
2= Smoking, 3= Inhalation, 
disabled,4= Injection (intravenous, intramuscular, intradermal, or subcutaneous), 5= 
Other]


-----------------------------------

ROUTE3 [Route of administration (tertiary), numeric, Scale] = This field identifies 
theusual route of administration of the corresponding substance identified in Substance 
Use(SUB3). 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Oral, 
2= Smoking, 3= Inhalation, 
disabled,4= Injection (intravenous, intramuscular, intradermal, or subcutaneous), 5= 
Other]


-----------------------------------

FREQ1 [Frequency of use at admission (primary), numeric, Scale] = Specifies the 
frequencyof use of the corresponding substance identified in Substance Use (SUB1). 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No use in the past month, 
2= Some use, 
3=Daily use]
 

-----------------------------------

FREQ2 [Frequency of use at admission (secondary), numeric, Scale] = Specifies the 
frequencyof use of the corresponding substance identified in Substance Use (SUB2). 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No use in the past month, 
2= Some use, 
3=Daily use]
 

-----------------------------------

FREQ3 [Frequency of use at admission (tertiary), numeric, Scale] = Specifies the 
frequencyof use of the corresponding substance identified in Substance Use (SUB3). 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No use in the past month, 
2= Some use, 
3=Daily use]
 

-----------------------------------

FREQ1_D [Frequency of use at discharge (primary), numeric, Scale] = Specifies the 
frequencyof use of the corresponding substance identified in Substance Use (SUB1) after 
discharge.


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No use in the past month, 
2= Some use, 
3=Daily use]
 

-----------------------------------

FREQ2_D [Frequency of use at discharge (secondary), numeric, Scale] = Specifies the 
frequencyof use of the corresponding substance identified in Substance Use (SUB2) after 
discharge.


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No use in the past month, 
2= Some use, 
3=Daily use]
 

-----------------------------------

FREQ3_D [Frequency of use at discharge (tertiary), numeric, Scale] = Specifies the 
frequencyof use of the corresponding substance identified in Substance Use (SUB3) after 
discharge.


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No use in the past month, 
2= Some use, 
3=Daily use]
 

-----------------------------------

FRSTUSE1 [Age at first use (primary), numeric, Scale] = For alcohol use, this is the age 
offirst intoxication. For substances other than alcohol, this field identifies the age 
atwhich the client first used the corresponding substance identified in Substance Use 
(SUB1) 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= 11 years and under, 2= 12-14 years, 
3=15-17years, 4= 18-20 years, 
5= 21-24 years, 6= 25-29 years, 7= 30 years and older]
 

-----------------------------------

FRSTUSE2 [Age at first use (secondary), numeric, Scale] = For alcohol use, this is the 
ageof first intoxication. For substances other than alcohol, this field identifies the 
ageat which the client first used the corresponding substance identified in Substance Use 
(SUB2).


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= 11 years and under, 2= 12-14 years, 
3=15-17years, 4= 18-20 years, 
5= 21-24 years, 6= 25-29 years, 7= 30 years and older]
 

-----------------------------------

FRSTUSE3 [Age at first use (tertiary), numeric, Scale] = For alcohol use, this is the age 
offirst intoxication. For substances other than alcohol, this field identifies the age 
atwhich the client first used the corresponding substance identified in Substance Use 
(SUB3).


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= 11 years and under, 2= 12-14 years, 
3=15-17years, 4= 18-20 years, 
5= 21-24 years, 6= 25-29 years, 7= 30 years and older]
 

-----------------------------------

HLTHINS [Health insurance, numeric, Scale] = 
This field specifies the client's 
healthinsurance at admission. The insurance may or may not cover behavioral health 
treatment.Reporting of this field is optional for both substance use and mental health clients. 
Statesare encouraged to report data for all categories in the list of valid entries, but 
reportinga subset of the categories is acceptable. Health insurance should be reported, if 
collected,whether or not it covers behavioral health treatment. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Private insurance, Blue Cross/Blue 
Shield,HMO, 2= Medicaid, 3= Medicare, other (e.g. TRICARE, CHAMPUS), 
4= None]
 

-----------------------------------

PRIMPAY [Payment source, primary (expected or actual), numeric, Scale] = This field 
identifiesthe primary source of payment for this treatment episode anticipated at the time of 
admission.
Guidelines:States operating under a split payment fee arrangement between multiple payment 
sourcesare to default to the payment source with the largest percentage. When payment 
percentagesare equal, the state can select either source. Reporting of this field is optional for 
bothsubstance use and mental health treatment clients. States are encouraged to report 
datafor all categories in the list of valid entries, but reporting a subset of the categories 
isacceptable. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= Self-pay, 2= Private insurance 
(BlueCross/Blue Shield, other health insurance, workers compensation), 
3= Medicare, 4= 
Medicaid,5= Other government payments, 
6= No charge (free, charity, special research, 
teaching),7= Other]
 

-----------------------------------

FREQ_ATND_SELF_HELP [Attendance at substance use self-help groups in past 30 days 
priorto admission, numeric, Scale] = This field indicates the frequency of attendance at a 
substanceuse self-help group in the 30 days prior to the reference date (the date of admission or 
dateof discharge). It includes attendance at Alcoholics Anonymous (AA), Narcotics 
Anonymous(NA), and other self-help/mutual support groups focused on recovery from substance 
useand dependence.
Guidelines: For admission records, the reference period is the 30 
daysprior to admission. The category '5: Some attendance' only applies if it is known that 
theclient attended a self-help program during the reference period, but there is 
insufficientinformation to assign a specific frequency. 


Variable Values = [-9= Missing/unknown/not collected/invalid, 1= No attendance, 2= 1-3 times in the 
pastmonth
, 3=4-7 times in the past month, 
4= 8-30 times in the past month, 5= Some 
attendance,frequency is unknown] 

-----------------------------------

FREQ_ATND_SELF_HELP_D [Attendance at substance use self-help groups in past 30 days 
priorto discharge, numeric, Scale] = This field indicates the frequency of attendance at a 
substanceuse self-help group in the 30 days prior to the discharge date (the date of discharge). It 
includesattendance at Alcoholics Anonymous (AA), Narcotics Anonymous (NA), and other 
self-help/mutualsupport groups focused on recovery from substance use and dependence.
Guidelines: 
Foradmission records, the reference period is the 30 days prior to discharge. The category 
'5:Some attendance' only applies if it is known that the client attended a self-help 
programduring the reference period, but there is insufficient information to assign a 
specificfrequency. 


Variable Values = 	[-9= Missing/unknown/not collected/invalid, 1= No attendance, 2= 1-3 times in the 
pastmonth
, 3=4-7 times in the past month, 
4= 8-30 times in the past month, 5= Some 
attendance,frequency is unknown] 

-----------------------------------

ALCFLG [Alcohol reported at admission, numeric, Nominal] = Flag records if alcohol was 
reportedas the primary, secondary, or tertiary substance at the time of admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

COKEFLG [Cocaine/crack reported at admission, numeric, Nominal] = Flag records if 
cocaineor crack was reported as the primary, secondary, or tertiary substance at the time of 
admission 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

MARFLG [Marijuana/hashish reported at admission, numeric, Nominal] = Flag records if 
marijuanaor hashish were reported as the primary, secondary, or tertiary substance at the time of 
admission


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

HERFLG [Heroin reported at admission, numeric, Nominal] = Flag records if heroin was 
reportedas the primary, secondary, or tertiary substance at the time of admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

METHFLG [Non-rx methadone reported at admission, numeric, Nominal] = Flag records if 
non-prescriptionmethadone was reported as the primary, secondary, or tertiary substance at the time of 
admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

OPSYNFLG [Other opiates/synthetics reported at admission, numeric, Nominal] = Flag 
recordsif other opiates or synthetics were reported as the primary, secondary, or tertiary 
substanceat the time of admission 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

PCPFLG [PCP reported at admission, numeric, Nominal] = Flag records if PCP was reported 
asthe primary, secondary, or tertiary substance at the time of admission 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

HALLFLG [Hallucinogens reported at admission, numeric, Nominal] = Flag records if 
hallucinogenswere reported as the primary, secondary, or tertiary substance at the time of admission 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

MTHAMFLG [Methamphetamine/speed reported at admission, numeric, Nominal] = Flag 
recordsif methamphetamine/speed was reported as the primary, secondary, or tertiary 
substanceat the time of admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

AMPHFLG [Other amphetamines reported at admission, numeric, Nominal] = Flag records 
ifother amphetamines were reported as the primary, secondary, or tertiary substance at 
thetime of admission.  


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

STIMFLG [Other stimulants reported at admission, numeric, Nominal] = Flag records if 
otherstimulants were reported as the primary, secondary, or tertiary substance at the time 
ofadmission 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

BENZFLG [Benzodiazepines reported at admission, numeric, Nominal] = Flag records if 
benzodiazepineswere reported as the primary, secondary, or tertiary substance at the time of 
admission.


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

TRNQFLG [Other tranquilizers reported at admission, numeric, Nominal] = Flag records 
ifother tranquilizers were reported as the primary, secondary, or tertiary substance at 
thetime of admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

BARBFLG [Barbiturates reported at admission, numeric, Nominal] = Flag records if 
barbiturateswere reported as the primary, secondary, or tertiary substance at the time of 
admission.


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

SEDHPFLG [Other sedatives/hypnotics reported at admission, numeric, Nominal] = Flag 
recordsif other sedatives or hypnotics were reported as the primary, secondary, or tertiary 
substanceat the time of admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

INHFLG [Inhalants reported at admission, numeric, Nominal] = Flag records if 
inhalantswere reported as the primary, secondary, or tertiary substance at the time of 
admission.


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

OTCFLG [Over-the-counter medication reported at admission, numeric, Nominal] = Flag 
recordsif over-the-counter medications were reported as the primary, secondary, or tertiary 
substanceat the time of admission. 


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

OTHERFLG [Other drug reported at admission, numeric, Nominal] = Flag records if other 
substanceswere reported as the primary, secondary, or tertiary substance at the time of 
admission.


Variable Values = [0= Substance not reported, 1= Substance reported] 

-----------------------------------

DIVISION [Census division, numeric, Nominal] = Census divisions are groupings of 
statesthat are subdivisions of the four Census regions. There are nine divisions, which the 
U.S.Census Bureau adopted in 1910 for the presentation of data. U.S. territories are not 
includedin any Census region or division. The divisions and the states included in them are:
• 
U.S.territories: Puerto Rico
• New England: Connecticut, Maine, Massachusetts, New 
Hampshire,Rhode Island, and Vermont.
• Middle Atlantic: New Jersey, New York, and 
Pennsylvania.
•East North Central: Illinois, Indiana, Michigan, Ohio, and Wisconsin.
• West North 
Central:Iowa, Kansas, Minnesota, Missouri, Nebraska, North Dakota, and South Dakota.
• South 
Atlantic:Delaware, District of Columbia, Florida, Georgia, Maryland, North Carolina, South 
Carolina,Virginia, and West Virginia.
• East South Central: Alabama, Kentucky, Mississippi, 
andTennessee.
• West South Central: Arkansas, Louisiana, Oklahoma, and Texas.
• 
Mountain:Arizona, Colorado, Idaho, Montana, Nevada, New Mexico, Utah, and Wyoming.
• Pacific: 
Alaska,California, Hawaii, Oregon, and Washington. 


Variable Values = [0= US jurisdiction/territory, 1= New England, 2= Middle Atlantic, 3= East North 
Central,
4= West North Central, 5= South Atlantic, 6= East South Central, 
7= West South 
Central,8= Mountain, 9= Pacific] 

-----------------------------------

REGION [Census region, numeric, Nominal] = Geographic regions used are based on 
divisionsused by the U.S. Census Bureau, with the addition of U.S. territories, which are not 
includedin any Census region:
• U.S. territories: Puerto Rico
• Northeast: New England 
Division(Connecticut, Maine, Massachusetts, New Hampshire, Rhode Island, Vermont) and 
MiddleAtlantic Division (New Jersey, New York, Pennsylvania).
• Midwest: East North 
CentralDivision (Illinois, Indiana, Michigan, Ohio, Wisconsin) and West North Central 
Division(Iowa, Kansas, Minnesota, Missouri, Nebraska, North Dakota, South Dakota).
• South: 
SouthAtlantic Division (Delaware, District of Columbia, Florida, Georgia, Maryland, 
NorthCarolina, South Carolina, Virginia, West Virginia), East South Central Division 
(Alabama,Kentucky, Mississippi, Tennessee), and West South Central Division (Arkansas, 
Louisiana,Oklahoma, Texas).
• West: Mountain Division (Arizona, Colorado, Idaho, Montana, 
Nevada,New Mexico, Utah, Wyoming) and Pacific Division (Alaska, California, Hawaii, Oregon, 
Washington).


Variable Values = [0= US jurisdiction/territory, 1= Northeast, 2= Midwest, 3= South, 4= West] 

-----------------------------------

IDU [Current IV drug use reported at admission, numeric, Scale] = Flag records if at 
leastone valid primary, secondary, or tertiary substance was reported and if injection was 
reportedamong the corresponding primary, secondary, or tertiary substances' route of 
administration.


Variable Values = [-9= No substances reported, 0= IDU not reported, 1= IDU reported]
 

-----------------------------------

ALCDRUG [Substance use type, numeric, Nominal] = Classifies client's substance use 
typeas alcohol only, other drugs only, alcohol and other drugs, or none. This variable looks 
acrossprimary, secondary, and tertiary substances reported at the time of admission to 
treatment.


Variable Values = [0= None, 1= Alcohol only, 2= Other drugs only, 3= Alcohol and other drugs]
 

-----------------------------------

