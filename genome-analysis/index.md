# Genome Analysis


I have long hesitated to order an analysis of my genome. I found it interesting to learn more about myself through genome. But on the other hand I am against the way in which cheap genome analysis companies try to use such personal data. In this article I explain how I had my genome analyzed, I give some advice to protect your personal data as much as possible, and how I proceeded to manually do a much deeper analysis of the raw genome data.

The scripts described in this article are on my repository: https://github.com/0p1um/genome-analysis

## Order a genome analysis

First of all, I think that only residents of the European Union should order these genomes analyzes. Because to my knowledge it is only in the European Union that we have laws that sufficiently protect personal data.

That said, I ordered a genome analysis kit from 23 and me, one of the leading companies in this field. I paid 100 dollars, and I got a kit, it's just a test tube to fill with saliva and send back. A few weeks later, genome analysis is available in the user area of the 23 and me website.

### Tips to protect protect your personal data

- Refuse that 23 and me keep your saliva sample.
- Refuse that your genome be compared with the genome of the other 23 and me clients to find genomic relatives. 
- Recover all the personal data that 23 and me has on you, including the analysis of your genome in raw format, it is this file that I analyze manually below.
- Once you have recovered your data, ask 23 and me to erase all your personal data.

## Raw genome analysis

### Generalities about genome

The genome is information.
The information is stored in 22 chromosomes, a chromosome is a string of characters encoding the information,
The information of the genome is coded in a biological language made up of molecules called nucleotide, there are 4 different nucleotide: A, C, G and T. The nucleotide can unite by complementarity, A with T, and C with G. In chromosomes (before cell division), there are 2 strings of nucleotides parallel to each other, each nucleotide in one of the strings is associated with its complementary nucleotide in the other string. It is like the strings are mirrored.
This is why these 2 nucleotide strings contain the same information.

Genetic information is similar to 99.9% for humans.
SNPs, "Single-nucleotide polymorphism", are possible variations of the nucleotide found at specific locations in the genome.

### Content of the raw genome file

The raw genome file provided by 23 and me is a simple text file, structured like a table whose columns are separated by tabs.

Example of a line contained in the file:

```
rs548049170     1       69869   TT
```

Each line corresponds to an SNP, and the elements of the line separated by tabs are:
- The SNP identifier, this identifier is used in scientific studies.
- The SNP chromosome.
- The position of the SNP in the chromosome. This position is given in the "positive" direction (one of the ways genomiic information is read)
- The variation of nucleotide that has been found in your genome.

### Analysis script

I made the script in python3 to find information about the SNPs in my raw genome file.

Run it with:
```
./get_snp_data.py <path_to_raw_23andme_genome_file>
```

The script will first get a list of all the SNPs that have a page on the site [snpedia] (http://snpedia.com/), it is a public site that compiles scientific information about SNPs.

Then, the genome file in argument of the script is read, and for each SNP contained in this file, if a page for it exists on [snpedia] (http://snpedia.com), the page is retrieved, and informations is extracted from it.

Note that if the SNP page uses the "minus" direction of genome reading, the result is modified to correspond with the "positive" direction as in the 23 and me genome file.

Finally, all this information is written as a json in the `snps_analysis.json` file.

### Script to read the analysis

I did another python3 script to read the json file generated previously in the command line.

Run the following command:
```
./format_snps_analysis.py <path_to_snps_analysis_json>
```

It will display summaries of what your SNPs correspond to, the value of SNPs, their identifiers, and the link [snpedia] (http://snpedia.com/) to their pages.

Sample of output from that script:
```
---------------

lower risk for skin cancer

rs1799782       GG      https://snpedia.org/index.php/Rs1799782(C;C)
---------------
```

