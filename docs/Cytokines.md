# Cytokine Data

### Dataset location

REDACTED


## Source

Cytokines were detected by Luminex assay using a combination of custom Milliplex panels and bespoke panels from Eve Technologies. Standard curves were fit and analyte concentrations were estimated as described in [Prolonged exposure to lung-derived cytokines is associated with inflammatory activation of microglia in patients with COVID-19](https://doi.org/10.1172/jci.insight.178859), including exclusion of low-confidence analyte predictions (saved as NA values).

There are a total of 793 samples.

## Data Description

The cytokine dataset contains the BAL or plasma sample ID and the corresponding cytokine protein expression values in [pg/mL]. 

Protein included are 
**CCL1	,CCL11	,CCL17	,CCL2	,CCL22	,CCL24	,CCL27	,CCL3	,CCL4	,CSF2	,CSF3	,CXCL1	,CXCL10	,CXCL13	,EGF	,FLT-3L	,IFNg	,IL-10	,IL-15	,IL-17A	,IL-1a	,IL-2	,IL-21	,IL-3	,IL-33	,IL-4	,IL-5	,IL-6	,IL-7	,IL-8	,IL-9	,IL1RN	,LIF	,LTA	,TGFa	,TNFa	,TRAIL	,TSLP	,VEGF-A	,CCL7	,CCL8	,IL-16	,IL-1b	,CCL15**\
The **sample_id** is linkable with other datasets including Flow cytometry, scRNAseq .etc


## From Grant et Poor et al.: Prolonged exposure to lung-derived cytokines is associated with inflammatory activation of microglia in patients with COVID-19

>In-house assays: Cytokine levels in matched BAL fluid and plasma collected from patients were measured using the multiplexed human cytokine/chemokine magnetic bead kits from Millipore (HCYTMAG-60K-PX41 and HCYP2MAG-62K) according to manufacturer’s protocol (HCYTOMAG-60K Rev. 18-MAY-2017). Briefly, frozen BAL and plasma samples were thawed, spun at 500rcf for 5 minutes to clarify, and 25uL of sample was added to 25uL of premixed magnetic beads (minus the beads for RANTES, PDGF-AA, and PDGF-AB/BB, which were excluded from analysis) in the provided 96 well plate, incubated at 4°C for 4 hours, washed, and then sequentially labeled with 25uL of detection antibodies followed by 25uL of streptavidin-phycoerythrin prior to analysis using a Luminex® 200 system. Raw MFI, bead counts, and standard concentrations were exported and analyzed as described below. CRO assays: Remaining assays were performed by Eve Technologies (Calgary, Alberta, Canada). Samples were thawed and aliquoted at 100µL, frozen and shipped to the CRO on dry ice. The Human Cytokine/Chemokine 71-Plex Discovery Assay (HD71) was then performed on each sample. Custom outputs containing raw MFI values, standard curve concentrations, and bead counts for processing as described below.


>Processing and high-level analysis were performed using custom scripts in R 4.1.1, which are included in the GitHub repository for ~this publication~ [Grant et Poor et al., 2024](https://github.com/NUPulmonary/2023_Grant_Poor). Raw MFI values, beads counts, and standard concentrations were first stripped from the data output from either Exponent (in-house assays; Luminex) or bespoke output from Eve Technologies (Calgary, Alberta, Canada). MFI measurements with fewer than 50 bead counts were discarded. Standard curves for each cytokine were then fit for each assay run using self-starting 5-parameter logistic (5PL) models using drc 3.2-076. Cutoffs for curves with low predictive value were then determined empirically using histograms MFI values vs standard concentrations to identify a bimodal distribution cutoff. For in-house assays, all values calculated using standard curves with MFI < 50 at 100pg/mL were discarded. For Eve Technologies assays, all values calculated using standard curves with MFI < 50 at 10pg/mL were discarded. Experimental values for each cytokine were then predicted using the ED function in drc with “absolute” value prediction. In rare cases where a 5PL model could not be hit for an individual cytokine-assay combination, these values were excluded (see Fig. S3). Values below the lower asymptote of the model were set to a concentration of 0pg/mL. Values above the upper asymptote were set to the value of the upper asymptote. Technical replicates (including those across assays) were collapsed by mean with NA values excluded. Plots of standard curves for each analyte for each assay were automatically exported and are available upon request. Analytes showing poor dynamic range, e.g. TNF-ɑ for all plasma samples, were excluded from further analysis. For calculations of cumulative exposure (AUC) by ICU day, a piecewise linear function was first fit for each patient for each analyte for all measurements during the patient’s stay using the approxfun function in R stats 4.1.1 using the “linear” method with n = 100. Initial measurements were carried out an additional day to represent the time of admission to first measurement, and final measurements were carried out an additional day to represent time until discharge or cessation of measurement. In rare cases where an initial measurement was missing, values were imputed by setting the initial measurement (day = 0) to the first measurement for the analyte/patient pair. Geometric integration was then performed using the integrate function from R stats 4.1.1 using the day of first measurement as the lower bound and the final day of measurement + 1 as the upper bound. Default parameters were used unless otherwise specified.
