# Automated-Matching-of-Patients-to-Clinical-Trials
Automated Matching of Patients to Clinical Trials: A patient-centric, Natural Language Processing Approach for Pediatric Leukemia

## Notebooks summary
Make sure Jupyter installed. You can try with the `jupyter --version` command. If it isn't please install it, one option is with pip `pip install jupyter jupyter-client jupyter-console jupyter-core`

Once installed run `jupyter notebook` in the directory containing the notebooks.

### criteria_classifer_derivation_final
Load the criteria embedding from `criteria_final_embedded.csv`.
For each unique label in the criteria file

### eligibility_criteria_extraction_final
Description:
- This notebook extracts individual-level eligibility criteria for all non-actively enrolling pediatric acute leukemia trials on ClinicalTrials.gov. Clinical trial protocols can be downloaded as XML files, and eligibility criteria can be extracted from these files as a single block of free text.
- Splitting each block of free text into individual criteria is not trivial, given that there is no single standardized format in which the criteria are listed. Criteria may appear as a bulleted list, bulleted list with related subbullets, outline, etc.
- The following steps were undertaken to tackle this problem:
216 XML files for all non-actively enrolling clinical trials for pediatric acute leukemia were downloaded from ClinicalTrials.gov, using the instructions listed at this link: https://clinicaltrials.gov/ct2/resources/download
  - The eligibility criteria free text block for each trial was manually inspected in order to ascertain patterns in formatting. The following four patterns emerged as dominant and each trial was manually labeled as such:

    - Major Header/Subheader/Subbullets:
      - DISEASE CHARACTERISTICS:
        - Criterion
        - Criterion
        - Etc
      - PATIENT CHARACTERISTICS:
        - Age
        - Criterion
        - Performance status
        - Criterion
        - SubCriterion
        - Etc
      - PRIOR CONCURRENT THERAPY:
        - Biologic therapy
        - Criterion
        - Etc

    - Major Header/Subbullets
      - DISEASE CHARACTERISTICS:
        - Criterion
        - Criterion
        - SubCriterion
        - Etc
      - PATIENT CHARACTERISTICS:
        - Criterion
        - Criterion
        - Etc
      - PRIOR CONCURRENT THERAPY:
        - Criterion
        - Criterion
        - Etc

    - Inclusion and/or Exclusion Criteria/No Nested Subbullets
      - Inclusion Criteria:
        - Criterion
        - Criterion
        - Etc
      - Exclusion Criteria:
        - Criterion
        - Criterion
        - Etc

    - Inclusion and Exclusion Criteria/Nested Subbullets
      - Inclusion Criteria:
        - Criterion
        - Criterion
          - SubCriterion
          - SubCriterion
          - Etc
      - Exclusion Criteria:
        - Criterion
        - Criterion
          - SubCriterion
          - SubCriterion
          - Etc

  - A function [ExtractCriteria()] was written to parse each text block according to its format and then extract as many intact criteria as possible.
  - NOTE: The goal of this process was not to extract each individual criterion perfectly, but to extract as many as feasible given the highly non-standardized nature of the free text blocks. Thus, a small number of criteria may be extracted as a large chunk, given that they have idiosyncratic structure or lack detectable common split character.

#### Notebook Format
- Running the notebook requires the folder of XML files ('trial_protocols').
- The notebook then can simply be run cell-by-cell from start to finish in order to reproduce the results.

### gearboxNLP
Expeceted input:
- embedding and classifier model that can be found in the trained_ML_models folder  
- A patient input in the form of a dictionary, where the attributes are key values of the patient characteristics. For instance the following
```
patient = {
           #patient's age in days
           "Age (Days)": 7000,
           
           #patient's height in cm
           "Height (cm)": 122,
           
           #patient's gender (female == True)
           "Female": True,
           
           #patient's self-identified race (included to calculate eGFR using MDRD equation)
           "African-American": False,
    
           #Lanksy or Karnofsky performance status scale (depending on age)
           "Performance Status (Lanksy/Karnofsky)": 50,
           
           #patient's diagnosis 
           "Diagnosis": 'Refractory AML',
           
           #CNS involvement scale
           "CNS Involvement (1/2/3)": 1,
           
           #presence of isolated CNS disease
           "Isolated CNS Disease": False,
           
           #days since prior therapies as integer - input False for treatments the patients has not had
           "Days Since Cytotoxic Chemotherapy": False,
           "Days Since Biologic Therapy": 50,
           "Days Since Growth Factor Therapy": 60,
           "Days Since Corticosteroids": 40,
           "Days Since Prior Radiotherapy": 10,
           
           #creatinine in mg/dL
           "Creatinine (mg/dL)": 1.0,
           
           #hepatic function times ULN - e.g. if AST and/or ALT is 2x ULN, input 2
           "AST/ALT Times ULN": 2,
           "Direct Bilirubin Times ULN": 2,
           
           #true if reduced EF, high anthracycline exposure, etc.
           "Impaired Cardiovascular Function/Cardiotoxicity from Chemotherapy": True,
           
           #true if patient current has an active infection (including HIV)
           "Active and/or Uncontrolled Viral, Bacterial, or Fungal Infection": True,
           
           #true if patient is pregnant, nursing, or fertile and unwilling to use contraception during the study
           "Pregnant, Nursing, or Fertile and Unwilling to Use Contraception": True}
```

- A list of trials in the form of either google docs or NCT ID's from clinicaltrials.gov trials

The NLPgearbox function will output the trials the patient is a potential match with.

Can you please expand on the README page to include more description of what is included in each notebook and file (e.g., where to find synthetic dataset creation)
