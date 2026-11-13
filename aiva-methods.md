# Methods description — MVA Hackathon 2026, Track 1 (Variant Prediction)

Please submit your answers with your Track 1 submission. All questions (with the exception of Question 1, in red) are voluntary; however, submissions without a methods description may be judged less favorably on the Innovation and Scalability criteria, since the panel can't assess what it can't see.

Fill out this form once per model/approach submitted.

## Team name

AIVA

## Model number (fill out this form once per model/approach submitted; up to 6)

1

## Please describe your model/approach in detail.

We use an agentic approach to query literature, our internal knowledge bases and other sources to identify potential pathogenic variants from patient's VCF. Below is the prompt we used -

```
Clinical Feature	HPO Term	HPO ID	Presentation / Notes
Rhabdomyosarcoma	Rhabdomyosarcoma	HP:0002859	Malignant soft tissue tumour: the primary oncological event that triggered urgent investigation
Nephrocalcinosis	Nephrocalcinosis	HP:0000121	Calcium deposits in kidney tissue; present since birth
Short stature	Short stature	HP:0004322	Height significantly below expected range for age and family background
Low muscle mass / Failure to thrive	Failure to thrive	HP:0001508	Documented repeatedly in medical records; poor weight gain and muscle development from early life
Low muscle mass / Failure to thrive	Skeletal muscle atrophy	HP:0003202	Reduced muscle bulk observed clinically alongside failure to thrive
Premature birth	Premature birth	HP:0001622	Born at 32 weeks gestation
Very low birth weight	Small for gestational age	HP:0001518	Approximately 1 kg at birth: severe low birth weight consistent with intrauterine growth restriction (IUGR)
Parental recurrent miscarriages	Recurrent spontaneous abortion	HP:0200067	Multiple pregnancy losses prior to the proband's birth: reported as maternal/parental history



can you use @playbook:Rare disease (Singleton)#44 on @sample:WGS_EX2312012_HGWCNDSX7 ? I want a table with top 10 candidates in this format - 

Field	Type	Description
proband_id	string	Proband identifier (provided in the dataset)
chrom_1	string	Chromosome of the first (or only) variant (e.g. chr15)
pos_1	integer	GRCh38 base-pair position of the first variant
ref_1	string	Reference allele of the first variant
alt_1	string	Alternate allele of the first variant
chrom_2	string	Chromosome of the second variant (if any); compound-het pairs only, blank otherwise
pos_2	integer	GRCh38 base-pair position of the second variant (blank if not applicable)
ref_2	string	Reference allele of the second variant (blank if not applicable)
alt_2	string	Alternate allele of the second variant (blank if not applicable)
epcr	float	Estimated probability of causal relationship, (0, 1]
finding_type	string	primary or secondary
notes	string	Optional - brief rationale, especially for secondary findings. For example, findings unrelated to the primary condition, but still worth surfacing
```

## (required) If commercially-available Generative AI was used, record the provider, the plan or tier, and the relevant setting in your methods description. A line is enough, for example, "Anthropic API, Claude Sonnet, commercial terms, no training on customer content."

Google Gemini API, Gemini 3.8 Flash, commercial terms, zero day retention enabled, no training on customer content

## Please state whether the submission file is the automated output of your computational approach (preferred), or if it has undergone downstream manual review and curation.

Automated output

## Please describe any downstream manual review in detail.

Updated the file name for submission

## Please state whether your approach only used publicly available data (preferred) or if proprietary data was also used. If proprietary data were used, where possible, please also include a submission based only on publicly available data.

Only public data

## Please describe any public data used in detail (e.g., reference panels, population databases, literature).

gnomAD, VEP cache, DITTO scores, pubmed literature

## Please describe any proprietary data used in detail.

NA

## Is your approach able to output proposed pairs of compound heterozygous candidate variants, or only single candidate variants?

yes

## How did you handle secondary or incidental findings, if any? (See the finding_type column in your submission.)

tagged them as secondary and scored lower than primary

## Please provide an estimate of run time and cost for your approach (if possible).

15min upload/annotation and 5min agentic analysis

## Provide a method abstract (up to 500 words). Please include strengths and limitations of your method as well as methodological detail.

AIVA is an agentic platform for rare disease variant interpretation. A large language model runs a plan-act-observe loop over genomics tools (VCF and BAM query, PubMed retrieval, an ACMG/AMP rule engine, a knowledge graph), retrieving gene-specific ClinGen VCEP specifications at analysis time and attaching citations programmatically from tool outputs, so every call is auditable. On 1,396 simulated rare disease cases, AIVA ranked the causal gene first in 66.7% of cases versus 59.5% for LIRICAL and 28.4% for Exomiser. On 8,387 ClinGen eRepo variants across 35 expert panels, it reached a macro F1 of 80.5% versus 75.3% for BIAS-2015 and 60.6% for InterVar.
