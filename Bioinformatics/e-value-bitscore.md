# E-values and Bit-scores in BLAST

- [E-values and Bit-scores in BLAST](#e-values-and-bit-scores-in-blast)
  - [E-value](#e-value)
  - [Bit-score](#bit-score)

## E-value 

The BLAST E-value is the number of expected hits of similar quality (score) that could be found just by chance. 

 

E-value of 10 means that up to 10 hits can be expected to be found just by chance, given the same size of a random database.  

 

E-value can be used as a first quality filter for the BLAST search result, to obtain only results equal to or better than the number given by the -evalue  option. Blast results are sorted by E-value by default (best hit in first line). 

 

    blastn -query genes.ffn -subject genome.fna -evalue 1e-10 

 

 

The smaller the E-value, the better the match. 

 

`-evalue 1e-50`    

small E-value: low number of hits, but of high quality 

Blast hits with an E-value smaller than 1e-50  includes database matches of very high quality. 

 

`-evalue 0.01`

Blast hits with E-value smaller than 0.01 can still be considered as good hit for homology matches. 

 

`-evalue 10   (default)` 

large E-value: many hits, partly of low quality 

E-value smaller than 10 will include hits that cannot be considered as significant, but may give an idea of potential relations. 

 
The E-value (expectation value) is a corrected bit-score adjusted to the sequence database size. The E-value therefore depends on the size of the used sequence database. Since large databases increase the chance of false positive hits,  the E-value corrects for the higher chance. It's a correction for multiple comparisons. This means that a sequence hit would get a better E-value when present in a smaller database.  


$E = m \cdot n  / 2^{bit-score}$ 

 

        $m$ - query sequence length

        $n$ - total database length (sum of all sequences) 

 

## Bit-score 

The higher the bit-score, the better the sequence similarity 

 

The bit-score is the requireds size of a sequence database in which the current match could be found just by chance. The bit-score is a log2 scaled and normalized raw-score. Each increase by one doubles the required database size (2bit-score). 

 

Bit-score does not depend on database size. The bit-score gives the same value for hits in databases of different sizes and hence can be used for searching in an constantly increasing database.  

 

From <http://www.metagenomics.wiki/tools/blast/evalue>  


---


The E-value provides information about the likelihood that a given sequence match is purely by chance. The lower the E-value, the less likely the database match is a result of random chance and therefore the more significant the match is. Empirical interpretation of the E-value is as follows. If E < 1e - 50 (or 1 × 10-50), there should be an extremely high confidence that the database match is a result of homologous relationships. If E is between 0.01 and 1e - 50, the match can be considered a result of homology. If E is between 0.01 and 10, the match is considered not significant, but may hint at a tentative remote homology relationship. Additional evidence is needed to confirm the tentative relationship. If E > 10, the sequences under consideration are either unrelated or related by extremely distant relationships that fall below the limit of detection with the current method. Because the E-value is proportionally affected by the database size, an obvious problem is that as the database grows, the E-value for a given sequence match also increases.  

A bit score is another prominent statistical indicator used in addition to the Evalue in a BLAST output. The bit score measures sequence similarity independent of query sequence length and database size and is normalized based on the rawpairwise alignment score. The bit score (S) is determined by the following formula: S = (λ × S − lnK)/ ln2 where λ is the Gumble distribution constant, S is the raw alignment score, and K is a constant associated with the scoring matrix used. Clearly, the bit score (S) is linearly related to the rawalignment score (S). Thus, the higher the bit score, the more highly significant the match is. The bit score provides a constant statistical indicator for searching different databases of different sizes or for searching the same database at different times as the database enlarges.  
it score provides a constant statistical indicator for searching different databases of different sizes or for searching the same database at different times as the database enlarges.  


From <https://www.biostars.org/p/187230/>  

---


https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3820096/ 

The bit-score provides a better rule-of-thumb for inferring homology. For average length proteins, a bit score of 50 is almost always significant. A bit score of 40 is only significant (E() < 0.001) in searches of protein databases with fewer than 7000 entries. Increasing the score by 10 b 


From <https://www.biostars.org/p/187230/>  

