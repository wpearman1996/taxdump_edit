# Taxdump Edit

## Why?
The taxdump files from NCBI, along with the 'nr' database, are often used in meta -genomics and -transcriptomics software to inform taxonomic identification of reads, contigs and ORFs. However, if you are working with organisms that have little to no representation in the NCBI databases then you may find yourself a bit stuck.

Many researchers in this situation will have custom databases of genomic/transcriptomic data and want to use it, but may still find their organism(s) unavailable within the NCBI taxonomy DB. If your organism does not have a valid TaxID in NCBI then you are unable to use many of the software packages that rely on 'taxdump' to extract taxonomic lineage and naming information with your custom DBs.

## What?
This tool will allow you to modify the 'taxdump' (names.dmp and nodes.dmp) files from NCBI, to temporarily include your organisms - until they find represenration of their own in the NCBI taxonomy lineage.

## How?
The script will automatically find the largest taxonomic ID in nodes.dmp and increment from that point (with a 10^length-1 addition) and assign it to your new taxa. This large addition is to avoid future conflicts with taxdump updates. If you are adding a group.... 

## Usage
```
	taxdump_edit.pl -names names.dmp -nodes nodes.dmp -taxa NAME -parent XXX -rank NAME -division X

	Required Input:
		-names names.dmp location
		-nodes nodes.dmp location
		-taxa new taxa/group name
		-parent parent TaxID
		-rank rank name (see -help)
		-division division ID (see -help)
	Optional Input
		-override TaxID from previous
	Optional Input (names.dmp):
		unique name
	Default Values (names.dmp):
		name class (scientific name) (see -help)
	Optional Input (nodes.dmp):
		embl code
		genetic code (1) (see -help)
		mitochondria genetic code (1) (see -help)
		comments
	Default Values (nodes.dmp):
		inherited div flag = 1
		inherited GC flag = 1
		inherited MGC flag = 1
		GenBank hidden flag = 1
		hidden subtree root flag = 1
```

### Variable Options
#### Divisions
	0 -> Bacteria
	1 -> Invertebrates
	2 -> Mammals
	3 -> Phages
	4 -> Plants and Fungi
	5 -> Primates
	6 -> Rodents
	7 -> Synthetic and Chimeric
	~~8 -> Unassigned - Do Not Use~~
	9 -> Viruses
	10 -> Vertebrates
	11 -> Environmental Samples
#### Genetic Code
	0 -> Unspecified
	1 -> Standard
	2 -> Vertebrate Mitochondrial
	3 -> Yeast Mitochondrial
	4 -> Mold Mitochondrial; Protozoan Mitochondrial; Coelenterate Mitochondrial; Mycoplasma; Spiroplasma
	5 -> Invertebrate Mitochondrial
	6 -> Ciliate Nuclear; Dasycladacean Nuclear; Hexamita Nuclear
	9 -> Echinoderm Mitochondrial; Flatworm Mitochondrial
	10 -> Euplotid Nuclear
	11 -> Bacterial, Archaeal and Plant Plastid
	12 -> Alternative Yeast Nuclear
	13 -> Ascidian Mitochondrial
	14 -> Alternative Flatworm Mitochondrial
	15 -> Blepharisma Macronuclear
	16 -> Chlorophycean Mitochondrial
	21 -> Trematode Mitochondrial
	22 -> Scenedesmus obliquus mitochondrial
	23 -> Thraustochytrium mitochondrial code
	24 -> Pterobranchia Mitochondrial
	25 -> Candidate Division SR 1 and Gracilibacteria
	26 -> Pachysolen tannophilus Nuclear
	27 -> Karyorelict Nuclear
	28 -> Condylostoma Nuclear
	29 -> Mesodinium Nuclear
	30 -> Peritrich Nuclear
	31 -> Blastocrithidia Nuclear
#### Name Class
	Acronym
	Anamorph
	Authority
	Blast Name
	Common Name
	Equivalent Name
	Genbank Acronym
	Genbank Anamorph
	Genbank Common Name
	Genbank Synonym
	Includes
	In-part
	Misnomer
	Misspelling
	Scientific Name
	Synonym
	Teleomorph
	Type Material
#### Taxonomic Rank
	no rank
	superkingdom
		kingdom
			subkingdom
	superphylum
		phylum
			subphylum
	superclass
		class
			subclass
				infraclass
	cohort
	superorder
		order
			suborder
				infraorder
					parvorder
	superfamily
		family
			subfamily
			tribe
				subtribe
		genus
			subgenus
	species group
		species
		species subgroup
			subspecies
				varietas
					forma

# More Information 
## Structure of \*.dmp files
As per NCBI's taxdump_readme.txt:
Each of the files store one record in the single line that are delimited by "\t|\n" (tab, vertical bar, and newline) characters. Each record consists of one or more fields delimited by "\t|\t" (tab, vertical bar, and tab) characters. The brief description of field position and meaning for each file follows.

## nodes.dmp
This file represents taxonomy nodes. The description for each node includes the following fields:

	tax_id					-- node id in GenBank taxonomy database
 	parent tax_id				-- parent node id in GenBank taxonomy database
 	rank					-- rank of this node (superkingdom, kingdom, ...) 
 	embl code				-- locus-name prefix; not unique
 	division id				-- see division.dmp file
 	inherited div flag  (1 or 0)		-- 1 if node inherits division from parent
 	genetic code id				-- see gencode.dmp file
 	inherited GC  flag  (1 or 0)		-- 1 if node inherits genetic code from parent
 	mitochondrial genetic code id		-- see gencode.dmp file
 	inherited MGC flag  (1 or 0)		-- 1 if node inherits mitochondrial gencode from parent
 	GenBank hidden flag (1 or 0)            -- 1 if name is suppressed in GenBank entry lineage
 	hidden subtree root flag (1 or 0)       -- 1 if this subtree has no sequence data yet
 	comments				-- free-text comments and citations

## names.dmp
Taxonomy names file has these fields:

	tax_id					-- the id of node associated with this name
	name_txt				-- name itself
	unique name				-- the unique variant of this name if name not unique
	name class				-- (synonym, common name, ...)

## Taxdump Files
```
wget ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/taxdump.tar.gz
tar zxvf taxdump.tar/gz
```

## Taxdump Readme
```
wget ftp://ftp.ncbi.nlm.nih.gov/pub/taxonomy/taxdump_readme.txt
```
