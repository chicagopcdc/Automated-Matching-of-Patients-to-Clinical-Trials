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


Can you please expand on the README page to include more description of what is included in each notebook and file (e.g., where to find synthetic dataset creation)
