## Here are some best practices for encoding protein sequences using BLOSUM62:

1. **Store the BLOSUM62 matrix as a 2D dictionary or array in your code**[5][6]. This allows you to easily look up the substitution score between any two amino acids.

2. **Iterate through each amino acid in the protein sequence**[6]. For each amino acid:
   - Look up its BLOSUM62 score row in the dictionary 
   - Concatenate that row as a vector to build up the encoding

3. **If sequences have different lengths, pad shorter ones with zeros** to a fixed length[1][6]. For example, pad TCR sequences to length 20 and epitopes to 22.

4. **Flatten the concatenated amino acid vectors into a single feature vector** to represent the entire protein sequence[1]. For example, a TCR sequence encoded with 24-dimensional BLOSUM62 vectors would result in a 480-dimensional feature vector.

5. **BLOSUM62 is a good default choice for encoding protein sequences**[1][2][4]. It captures evolutionary relationships between amino acids and has been widely used for TCR and epitope analysis.

6. **Other encoding options include one-hot encoding, learned embeddings from language models like ProtBERT, and concatenating physicochemical property vectors**[1][3]. But BLOSUM62 is simple, effective, and commonly used.

7. **Avoid extrapolating BLOSUM matrices to very distant relationships**[2]. Use BLOSUM matrices with higher numbers (e.g. BLOSUM80) for closely related sequences, and lower numbers (e.g. BLOSUM45) for more distant relationships.

8. **Combine BLOSUM62 encoding with other features like sequence motifs, structural properties, etc. to improve model performance**[3].

In summary, BLOSUM62 is a compact, effective way to encode protein sequences that captures evolutionary information. Storing the matrix in code, padding sequences to a fixed length, and flattening the concatenated amino acid vectors are some best practices. But also consider other encoding options and combining BLOSUM62 with additional features.

Citations:
[1] https://elifesciences.org/reviewed-preprints/88837v1
[2] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3848038/
[3] https://www.biostars.org/p/207566/
[4] https://en.wikipedia.org/wiki/BLOSUM
[5] https://stackoverflow.com/questions/2380650/coding-blosum62-in-the-source-code
[6] https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-020-03546-x
[7] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6337344/
[8] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6343287/
[9] https://www.nielsvandervelden.com/blog/2022-06-15-protein-encoding-for-deep-learning-models/
[10] https://github.com/JJAlmagro/TargetP-2.0/issues/1
[11] https://www.sciencedirect.com/topics/biochemistry-genetics-and-molecular-biology/blosum




## Why padding is required for Blossom62

Padding of TCR and epitope sequences is required when using BLOSUM62 encoding because the sequences can have different lengths, but the encoding requires a fixed-length input[1][3][5].

Here's why padding is necessary:

1. **BLOSUM62 encodes each amino acid as a 24-dimensional vector**[1]. To represent an entire TCR or epitope sequence, these 24D vectors are concatenated.

2. **However, TCR and epitope sequences can vary in length**[1][2]. For example, TCRs may be 10-20 amino acids long, while epitopes are typically 8-11 amino acids.

3. **To create a fixed-length input for the model**, shorter sequences need to be padded with extra values (usually zeros)[1][3][5]. For example, padding TCRs to length 20 and epitopes to 22.

4. **Without padding, the concatenated BLOSUM62 vectors would have different lengths for each sequence**[1][6]. This would prevent them from being used as inputs to machine learning models, which require fixed-length feature vectors.

5. **Padding ensures that all sequences are represented by the same number of concatenated BLOSUM62 vectors**, allowing them to be used as inputs to the model[1][3][5].

So in summary, padding is a necessary preprocessing step when using BLOSUM62 encoding for TCR and epitope sequences, as it allows converting variable-length sequences into a fixed-length format required by machine learning models. The specific padding lengths are chosen based on the typical ranges of TCR and epitope sequence lengths.

Citations:
[1] https://elifesciences.org/reviewed-preprints/88837v1
[2] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC9299376/
[3] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8275323/
[4] https://academic.oup.com/bib/article/22/4/bbaa318/6042663
[5] https://academic.oup.com/bioinformatics/article/38/Supplement_1/i246/6617535
[6] https://www.frontiersin.org/journals/immunology/articles/10.3389/fimmu.2020.01803/full
[7] https://www.frontiersin.org/journals/immunology/articles/10.3389/fimmu.2023.1228873/full
[8] https://icml-compbio.github.io/icml-website-2021/2021/papers/WCBICML2021_paper_59.pdf


## Why should we pad the Epitope and TCRs

To showcase padding TCR and epitope sequences to a fixed length using zeros, let's use the example TCR sequence 'CAISEYPPTGRKADTQYF' provided:

1. **Determine the desired fixed length for each sequence type**[1][6]. Let's say we want to pad TCR sequences to length 20 and epitopes to 22.

2. **Iterate through the TCR sequence 'CAISEYPPTGRKADTQYF'**[6]. The length is 18 amino acids. Since this is shorter than the desired length of 20, we need to pad it.

3. **Append zeros to the end of the TCR sequence** until it reaches length 20[1][6]:

   'CAISEYPPTGRKADTQYF00'

4. **For the epitope sequence, no padding is needed** since the length of 18 is already less than the desired length of 22.

So the final padded sequences would be:

TCR: 'CAISEYPPTGRKADTQYF00' (length 20)  
Epitope: 'CAISEYPPTGRKADTQYF' (length 18, no padding needed)

The key points are[1][6]:

- Determine a fixed length to pad all sequences of the same type to 
- Append zeros to the end of shorter sequences to reach the fixed length
- This allows using batches of sequences with different lengths as input to deep learning models

Some other padding options include[1][5]:

- Padding with zeros at the beginning instead of the end
- Distributing the padding zeros uniformly across the sequence 
- Repeating the sequence itself instead of using zeros

But appending zeros to the end is the most common and straightforward approach. Let me know if you have any other questions!

Citations:
[1] https://www.nature.com/articles/s41598-020-71450-8
[2] https://academic.oup.com/bioinformatics/article/38/8/2102/6502274
[3] https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-020-03546-x
[4] https://www.frontiersin.org/articles/10.3389/fbioe.2020.00391/full
[5] https://www.researchgate.net/figure/Different-types-of-padding-analysed-in-this-study-Each-color-bin-represents-an-amino_fig1_344086945
[6] https://www.researchgate.net/publication/340196841_Effect_of_Sequence_Padding_on_the_Performance_of_Protein-Based_Deep_Learning_Models
[7] https://www.pnas.org/doi/full/10.1073/pnas.2104878118
[8] https://www.sciencedirect.com/science/article/pii/S1532046422000326