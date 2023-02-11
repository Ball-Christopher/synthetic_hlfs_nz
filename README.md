# Synthetic Household Labour Force Survey (HLFS) data for New Zealand

Christopher Ball
chrispball1@gmail.com

These data are synthetic, and are NOT the real Household Labour Force Survey data.  This statement is required under Section 4.15.3 of the [Microdata Output Guide Version 5.2](https://www.stats.govt.nz/assets/Methods/Microdata-Output-Guide-2020-v5-Sept22update.pdf), and this statement must be distributed along with any use of these databases.

## Data access

Data is stored in [fst format](https://www.fstpackage.org/), a compressed data format that can be accessed with the R programming language.  The following code snippet shows how to load the fst files into R using the fst package and the read_fst function, as well as how to subsequently save the files into csv format.  Note that this assumes that both the data.table and fst paqckages are installed, and that the working directory contains the fst files.

    library(data.table)
    library(fst)
    
    dt <- read_fst('synthetic_hlfs_2016.fst', as.data.table = TRUE)
    fwrite(dt, 'synthetic_hlfs_2016.csv')


## Data description

| Column name            | Description | Values |
|------------------------|-------------|--------|
| id_person              | Anonymised person identifier.  Specific to each database, so id_person = 1 in 2016 is a different person to id_person = 1 in 2017.  Values are unique for each database (one record per person).  All records are adults.  |   1 to N, where N is the number of records in each database.     |
| id_family              |  Anonymised family identifier.  Specific to each database, so id_family = 1 in 2016 is a different family to id_family = 1 in 2017.  A family consists of an adult principal (male of female), potentially an adult spouse (male or female) and any number of dependent children aged 0 to 18 (including zero children).  There are at most two records for each family, for the principal and spouse (if present).  The count of children by age band are summarised in columns children_00_04, children_05_09, children_10_14 and children_15_18.  No further information about the children is provided in these synthetic data.  |  1 to M, where M is the number of families in each database.    |
| family_type            |   Classifies the family into one of 6 family types: Single Male, Single Female, Sole Parent Female, Sole Parent Male, "Couple, no children" and Couple with children.  These 6 categories cover all possibilities due to the definition of a family used.  |  Single Male, Single Female, Sole Parent Female, Sole Parent Male, "Couple, no children" and Couple with children      |
| multi_family_household |   Binary indicator of whether the family lives in a multi-family household.  Some examples of where a multi-family household can occur include multi-generational households, student flats, boarders, and adult(s) living with their parent(s),          |  0 for single family household, 1 for multi-family household.      |
| weight                 |   A number that represents how many individuals in New Zealand the sample unit represents.  **Almost every use of these databases should use the weight column [for weighted means, weighted counts, weighted inequality measures, ...]**     |  A number between 20 and 1,000, with 3 decimal places.      |
| hours_total            |   The total number of hours per week which the individual works across wage & salary and self-employed jobs.  Based on survey responses.  |  A number between 0 and 7*24  inclusive.    |
| hours_wage_salary      |   The number of hours per week which the individual works at wage & salary jobs. Based on survey responses.  |  A number between 0 and 7*24  inclusive.  |
| wage                   |   The hourly wage rate for the individual.  Based on survey responses for those employed, and imputed for individuals who are not employed.  The wage rate can be negative for self-employed, and in rare cases the wage rate can be below the applicable minimum wage for wage & salary employees (in broadly the same proportion to the percentage of survey responses which are below the minimum wage).          |  A number, usually positive.  |
| children_00_04         |  The number of children aged between 0 and 4.  This variable is the same for each adult in the family.           |  A whole number (which includes 0)      |
| children_05_09         |  The number of children aged between 5 and 9.  This variable is the same for each adult in the family.           | A whole number (which includes 0)        |
| children_10_14         |  The number of children aged between 10 and 14.  This variable is the same for each adult in the family.           | A whole number (which includes 0)        |
| children_15_18         |  The number of **dependent** children aged between 15 and 18.  Individuals aged 19 or older are adults.  Individuals aged 15 to 18 can be either dependent children captured in this variable for their parents, or independent adults with their own separate "family" in the database.  Dependency status is determined by considering a combination of employment status, study status, presence of partner and presence of parents.  This variable is the same for each adult in the family.           | A whole number (which includes 0)        |
| living_with_x_parents  |  This variable indicates how many parents of the reference individual live in the same household.  Note that this can differ for each adult member of the couple.  Based on survey reported person-to-person relationships in the household, where parent is "in a parent role" and may not necessarily refer to relationships by blood.  |  0, 1 or 2.  |
| region                 |  The Stats NZ benchmarking region in which this household is located.  Note that these 12 benchmarking regions differ from other region concepts.  |  "Auckland", "Bay of Plenty", "Canterbury", "Gisborne/Hawke's Bay", "Manawatu-Wanganui", "Nelson/Tasman/Marlborough/West Coast", "Northland", "Otago", "Southland", "Taranaki", "Waikato", "Wellington"  |
| age         |  Age of individual when surveyed.           |  Number greater than 15 (min age for adult).  |
| gender         |  Gender indicator for male or female.  Non-binary adults are not included due to small sample sizes.  | 1 - Male, 2 - Female       |
| ethnicity_asian         |  Multiple response indicator of high-level ethnicity Asian.  An individual may select any combination of the ethnicity indicators.           |   0 - Not Asian, 1 - Asian     |
| ethnicity_european         |  Multiple response indicator of high-level ethnicity European.  An individual may select any combination of the ethnicity indicators.           |   0 - Not European, 1 - European   |
| ethnicity_maori  | Multiple response indicator of high-level ethnicity Maori.  An individual may select any combination of the ethnicity indicators.           |   0 - Not Maori, 1 - Maori   |
| ethnicity_melaa   |  Multiple response indicator of high-level ethnicity Middle Eastern, Latin American or African (MELAA).  An individual may select any combination of the ethnicity indicators.           |   0 - Not Middle Eastern, Latin American or African (MELAA), 1 - Middle Eastern, Latin American or African (MELAA)  |
| ethnicity_other |    Multiple response indicator of high-level ethnicity Other.  An individual may select any combination of the ethnicity indicators.  Other ethnicity according to Ethnicity New Zealand Standard Classification 2005 V2.1.0 includes Indigenous American, Mauritian, Seychellois, Other South African, New Zealander and Other Ethnicity not elsewhere classified.  |  0 - Not Other Ethnicity, 1 - Other Ethnicity |
| ethnicity_pasifika |  Multiple response indicator of high-level ethnicity Pacific Peoples.  An individual may select any combination of the ethnicity indicators.           |   0 - Not Pacific Peoples, 1 - Pacific Peoples  |
| highest_qualification  |  Ordinal variable summarising the individual's highest qualification attainment.           |  "00 - No Qualifications (22, 99)",  "01 - Level 1 Equivalent (19, 20, 21)", "02 - Level 2 Equivalent (12, 13, 14, 17, 18)",  "03 - Level 3 Equivalent (15, 16)", "04 - Post-level 3 < Bachelors",  "05 - Bachelors", "06 - Bachelors (Honours)", "07 - Masters",  "08 - PhD"      |
| extended_labour_force_status  |   The individual's labour force status, with further splits of 1) the employed population into employed (not underemployed), underemployed and seeking a new job, and underemployed and not seeking a new job; and 2) the Not In Labour Force (NILF) population into NILF Active (searching and not available OR available and not searching), NILF potential (not searching due to too old, too young, not enough experience or due to child care or care of others - broadly the group who could potentially join the labour market with a good enough opportunity) and NILF out (not employed and unlikely to return to the labour market due to 65+, sick/disabled, retired, etc). |  "1 - Employed", "2 - Underemployed, seeking",  "3 - Underemployed, not seeking", "4 - Unemployed", "5 - NILF Active",  "6 - NILF Potential", "7 - NILF Out"      |
| employment_type |  Employment type.           |  "10 - Not working", "11 - Paid Employee", "12 - Employer", "13 - Self-employed"      |
| days_worked  |  Number of days worked per week.           |  0 to 7.      |
| industry |  Level 1 industry of employment.  Imputed for those not employed.           |   "A - Agriculture, Forestry and Fishing", "B - Mining", "C - Manufacturing", "D - Electricity, Gas, Water and Waste Services", "E - Construction", "F - Wholesale Trade", "G - Retail Trade", "H - Accommodation and Food Services", "I - Transport, Postal and Warehousing", "J - Information Media and Telecommunications", "K - Financial and Insurance Services", "L - Rental, Hiring and Real Estate Services", "M - Professional, Scientific and Technical Services", "N - Administrative and Support Services", "O - Public Administration and Safety", "P - Education and Training", "Q - Health Care and Social Assistance", "R - Arts and Recreation Services", "S - Other Services", "T - Not Elsewhere Included"     |
| occupation |   Level 1 occupation of employment.  Imputed for those not employed.          |  "1 - Managers", "2 - Professionals", "3 - Technicians and Trades Workers", "4 - Community and Personal Service Workers", "5 - Clerical and Administrative Workers", "6 - Sales Workers", "7 - Machinery Operators and Drivers", "8 - Labourers", "9 - Residual Categories"      |
| number_of_jobs |  Number of jobs held by individual.  Capped at 3.           |  0, 1, 2 or 3      |
| study_status |  Study status of individual           |  "01 - Formal study", "02 - Informal study", "04 - No study"      |
| received_accommodation_supplement  |  Indicator of receiving Accommodation Supplement.  Based on administrative records of receipt.           |     0 - Did not receive, 1 - Received   |
| received_childcare_assistance |  Indicator of receiving Child Care Assistance from MSD.  Based on administrative records of receipt.           |     0 - Did not receive, 1 - Received  |
| received_job_seeker_support | Indicator of receiving Job Seeker Support.  Based on administrative records of receipt.           |     0 - Did not receive, 1 - Received  |
| received_supported_living_payment |  Indicator of receiving Supported Living Payment.  Based on administrative records of receipt. | 0 - Did not receive, 1 - Received |
| received_sole_parent_support |  Indicator of receiving Sole Parent Support.  Based on administrative records of receipt.  | 0 - Did not receive, 1 - Received |
| received_working_for_families  |  Indicator of receiving Working for Families (Family Tax Credit or In Work Tax Credit).  This indicator may miss a small number of families who received the Best Start (or the defunct Parental Tax Credit) and did not receive FTC or IWTC.  Based on administrative records of receipt.  | 0 - Did not receive, 1 - Received  |

## Disclaimer

Access to the raw data was provided by Stats NZ under conditions designed to give effect to the security and confidentiality provisions of the Data and Statistics Act 2022. These synthetic data sets are the work of the author, not Stats NZ or individual data suppliers. 

These results are not official statistics. They have been created for research purposes from the Integrated Data Infrastructure (IDI) which is carefully managed by Stats NZ. For more information about the IDI please visit https://www.stats.govt.nz/integrated-data/.

The results are based in part on tax data supplied by Inland Revenue to Stats NZ under the Tax Administration Act 1994 for statistical purposes. Any discussion of data limitations or weaknesses is in the context of using the IDI for statistical purposes, and is not related to the data’s ability to support Inland Revenue’s core operational requirements.

These data are synthetic, and are NOT the real Household Labour Force Survey data.  This statement is required under Section 4.15.3 of the [Microdata Output Guide Version 5.2](https://www.stats.govt.nz/assets/Methods/Microdata-Output-Guide-2020-v5-Sept22update.pdf), and this statement must be distributed along with any use of these databases.


## License

For the avoidance of doubt, the following license applies if and only if the disclaimer is included with these databases.  These databases cannot be used or distributed without the disclaimer.

This Synthetic Household Labour Force Survey (HLFS) data for New Zealand is made available under the Open Database License: http://opendatacommons.org/licenses/odbl/1.0/. Any rights in individual contents of the database are licensed under the Database Contents License: http://opendatacommons.org/licenses/dbcl/1.0/.
