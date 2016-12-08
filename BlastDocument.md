## 1. Programs available for the BLAST search**

## 可用于BLAST搜索的程序

The NCBI BLAST family of programs includes: 
**blastp:** compares an amino acid query sequence against a protein sequence database 
**blastn:** compares a nucleotide query sequence against a nucleotide sequence database 
**blastx:** compares a nucleotide query sequence translated in all reading frames against a protein sequence database 
**tblastn:** compares a protein query sequence against a nucleotide sequence database dynamically translated in all reading frames 
**tblastx:** compares the six-frame translations of a nucleotide query sequence against the six-frame translations of a nucleotide sequence database.

NCBI BLAST程序包括：

blastp：将氨基酸查询序列与蛋白质序列数据库进行比对

blastn：将核苷酸查询序列与核苷酸序列数据库进行比对

blastx：核酸序列对蛋白质库的比对，核酸序列在比对之前自动按照六个读码框翻译成蛋白质序列

tblastn：蛋白质序列对核酸库的比对，核酸库中的序列按照六个读码框翻译后与蛋白质序列进行比对搜索

tblastx：核酸序列对核酸库在蛋白质级别的比对，两者都在搜索之前翻译成为蛋白质质进行比对

## 2. FASTA format description

## FASTA格式描述

A sequence in FASTA format begins with a single-line description, followed by lines of sequence data. The description line is distinguished from the sequence data by a greater-than (">") symbol in the first column. It is recommended that all lines of text be shorter than 80 characters in length. An example sequence in FASTA format is:

FASTA格式的序列以单行描述信息开始，后跟序列数据行。描述行通过第一列中的大于（“>”）符号与序列数据分开。  建议所有文本行的长度小于80个字符。 FASTA格式的示例序列为：

Sequences are expected to be represented in the standard IUB/IUPAC amino acid and nucleic acid codes, with these exceptions: lower-case letters are accepted and are mapped into upper-case; a single hyphen or dash can be used to represent a gap of indeterminate length; and in amino acid sequences, U and * are acceptable letters (see below). Before submitting a request, any numerical digits in the query sequence should either be removed or replaced by appropriate letter codes (e.g., N for unknown nucleic acid residue or X for unknown amino acid residue). The nucleic acid codes supported are:

序列参照IUB/IUPAC氨基酸和核酸编码标准，除去：小写字符会全部转换成大写；单个“-”号代表不明长度的空位；在氨基酸序列里允许出现“U”和“*”号（见下文）；在提交比对序列时，数字都应该被去掉或换成字母(如，不明确的核酸用“N”，不明确的氨基酸用“X”)。所支持的核酸代码是：

For those programs that use amino acid query sequences (BLASTP and TBLASTN), the accepted amino acid codes are:

对于使用氨基酸查询序列的程序（BLASTP和TBLASTN），接受的氨基酸代码是：

## 3. Low complexity filtering

## 低复杂度过滤

The server filters your query sequence for low compositional complexity regions by default. Low complexity regions commonly give spuriously high scores that reflect compositional bias rather than significant position-by- position alignment. Filtering can elminate these potentially confounding matches (e.g., hits against proline-rich regions or poly-A tails) from the blast reports, leaving regions whose blast statistics reflect the specificity of their pairwise alignment. Queries searched with the blastn program are filtered with DUST. Other programs use SEG.

默认情况下，比对程序会根据低组合复杂度区域筛选查询序列。序列中的低复杂度区域会导致非碱基匹配情况下的比对结果高分。 过滤功能可以从blast结果中分离这些潜在的混杂匹配（例如，对富含脯氨酸的区域或多聚腺苷酸尾的匹配），保留在统计水平上能够反映碱基成对比对的特异性区域。 使用blastn程序进行比对会用DUST算法过滤，而其他比对程序会使用SEG。

Low complexity sequence found by a filter program is substituted using the letter "N" in nucleotide sequence (e.g., "NNNNNNNNNNNNN") and the letter "X" in protein sequences (e.g., "XXXXXXXXX"). Users may turn off filtering by using the "Filter" option on the "Advanced options for the BLAST server" page.

使用核苷酸序列（例如“NNNNNNNNNNNNNN”）中的字母“N”和蛋白质序列（例如，“XXXXXXXXX”）中的字母“X”替换由过滤程序发现的低复杂度序列。

##### **Reference for the DUST program:**

DUST程序的参考文件：

##### **Reference for the SEG program:**

SEG程序的参考文献：

##### Reference for the role of filtering in search strategies:

关于搜索策略使用过滤算法的参考文献：

## 4. Out-Of-Frame BLAST notation 
## 框外BLAST表示法

When protein aligned to the nucleotide there are 6 possibilities of match at any point. In OOF alignment - upper sequence is DNAP - 3-frame translated DNA. Lower sequence is protein. At any position next protein base may be aligned to 6 possible bases in DNAP:

当蛋白质与核苷酸比对时，~~在任何点有6种匹配的可能性~~ 每一个点的匹配都有6种可能。 在OOF比对中 - 上游序列是由DNAP-3-frame翻译成的DNA序列。 ~~较低的~~ 下游的序列是蛋白质。 ~~在任何位置，下一个蛋白质碱基可以与DNAP中的6种可能的碱基比对~~ 在任何位置，下一个蛋白碱基都有可能比对到 DNAP 中 6 种可能的碱基上：

0: 3 nucleotides missing - gap (TBO notation "-")

0：缺失3个核苷酸 - 缺口（TBO 符号“ - ”）

1: 2 nucleotides missing - "frameshift -2" (TBO notation "\\")

1: 缺失2个核苷酸 - "移码-2"（TBO 符号 "\\"）

2: 1 nucletide missing - "frameshift -1" (TBO notation "\")

2：缺失1个核苷酸 - "移码-1"（TBO 符号 "\"）

3: Complete match

3：完全匹配

4: 1 nucleotide insertion - "frameshift +1" (TBO notation "/")

4：~~1个核苷酸插入~~ 插入1个核苷酸 - "移码+1"（TBO 符号 "/"）

5: 2 nucleotides insertion - "frameshift +2" (TBP notation "//")

5: 2个核苷酸插入 - "移码 + 2"（TBP 符号 "//"）

## 5. BLAST Search main parameters

## BLAST 搜索主要参数

**ALIGNMENTS:**

**比对:**

Restricts database sequences to the number specified for which high-scoring segment pairs (HSPs) are reported; the default limit is 100\. If more database sequences than this happen to satisfy the statistical significance threshold for reporting (see EXPECT below), only the matches ascribed the greatest statistical significance are reported.

将数据库序列限制为比对结果报告中高评分片段对（HSPs）的数量，默认限制为100。如果更多的数据库序列不足以满足报告的~~统计显着性阈值~~ 统计显著性阈值（见下面的EXPECT），则只 ~~报告~~ 记录具有最大统计意义的匹配。

**EXPECT:**

The statistical significance threshold for reporting matches against database sequences; the default value is 10, such that 10 matches are expected to be found merely by chance, according to the stochastic model of Karlin and Altschul (1990). If the statistical significance ascribed to a match is greater than the EXPECT threshold, the match will not be reported. Lower EXPECT thresholds are more stringent, leading to fewer chance matches being reported. Fractional values are acceptable.

~~比对结果与数据库序列匹配的统计显着性阈值~~ 用于报告与数据库序列匹配的统计显著性阈值; 默认值为10，根据 Karlin 和Altschul（1990）的随机模型，~~预期仅仅偶然发现10个匹配~~ 比对结果中由于随机偶然性产生的匹配结果不大于10。 如果匹配的统计 ~~显着性~~ 显著性大于 EXPECT 阈值，~~则不会有匹配~~ 匹配则不会被报导。 较低的 EXPECT 阈值更严格，导致比对结果的匹配数较少。小数值是可以接受的。

**FILTER (Low-complexity)**

**过滤：(低复杂度)**

Mask off segments of the query sequence that have low compositional complexity, as determined by the SEG program of Wootton & Federhen (Computers and Chemistry, 1993) or, for BLASTN, by the DUST program of Tatusov and Lipman (in preparation). Filtering can eliminate statistically significant but biologically uninteresting reports from the blast output (e.g., hits against common acidic-, basic- or proline-rich regions), leaving the more biologically interesting regions of the query sequence available for specific matching against database sequences.

~~掩蔽~~ 屏蔽具有低组成复杂性的查询序列 ~~的~~ 片段，~~如通过~~ 这取决于 Wootton＆Federhen（Computers and Chemistry，1993）的SEG程序 ~~确定的~~，而对于BLASTN, ~~通过~~则取决于 Tatusov 和 Lipman（~~在制备中~~ 开发准备中）的DUST程序 ~~确定~~。 过滤可以消除来自blast输出结果中具有统计学~~显着的~~显著但是生物学上~~不感兴趣~~ 没有意义的报告（例如，对常见的富含酸性，富含碱性或富含脯氨酸区域的匹配），留下~~查询序列的~~用于与数据库序列进行特异性匹配，更具生物学意义区域的查询序列。

Filtering is only applied to the query sequence (or its translation products), not to database sequences. Default filtering is DUST for BLASTN, SEG for other programs.

过滤仅应用于查询序列（或其~~翻译产品~~翻译产物），而不应用于数据库序列。 ~~对于BLASTN，SEG为其他程序默认过滤为DUST~~ 对于BLASTN,默认使用DUST过滤，其他程序则默认SEG过滤。

It is not unusual for nothing at all to be masked by SEG, when applied to sequences in SWISS-PROT, so filtering should not be expected to always yield an effect. Furthermore, in some cases, sequences are masked in their entirety, indicating that the statistical significance of any matches reported against the unfiltered query sequence should be suspect.

当应用于 SWISS-PROT 中的序列时，SEG不~~掩蔽~~ 屏蔽任何东西 ~~并不奇怪~~ 有时候也是正常的，因此不应期望过滤总是产生效果。 此外，在一些情况下，序列被完全屏蔽，~~指示~~ 这表明针对未过滤的查询序列报告的任何匹配的统计~~显着性~~ 显著性应该是可疑的。

**FILTER (Human repeats)**

**过滤(~~人类重复~~人类重复序列)**

This option masks Human repeats (LINE's and SINE's) and is especially useful for human sequences that may contain these repeats. This option is still experimental and under development, so it may change in the near future.

此选项掩盖~~人类重复~~人类重复序列（LINE's 和 SINE's），并且对可能包含这些重复片段的人类序列~~特别有用~~具有特别的作用。 该选项仍然是具有实验性的、正在开发中的，因此它可能在不久的将来~~改变~~有所更改。

**FILTER (Mask for lookup table only)**

**过滤(仅适用于查找表的掩码)**

This option masks only for purposes of constructing the lookup table used by BLAST. The BLAST extensions are performed without masking. This option is still experimental and may change in the near future.

此选项的目的是仅用于构建BLAST使用的查找表。 BLAST~~扩展~~的扩展程序可以在在没有掩蔽的情况下执行。 此选项仍处于实验阶段，可能会在不久的将来更改。

**Word-size:**

**~~字数~~ 字长大小：**

BLAST is a heuristic that works by finding word-matches between the query and database sequences. One may think of this process as finding "hot-spots" that BLAST can then use to initiate extensions that might eventually lead to full-blown alignments. For nucleotide-nucleotide searches (i.e., "blastn") an exact match of the entire word is required before an extension is initiated, so that one normally regulates the sensitivity and speed of the search by increasing or decreasing the word-size. For other BLAST searches non-exact word matches are taken into account based upon the similarity between words. The amount of similarity can be varied. The webpage allows the word-sizes 2, 3, and 6.

BLAST是一种~~启发式方法~~启发式的程序，通过查找~~查询~~查询序列和数据库序列之间词的匹配来工作。 人们可能认为这个过程是找到"~~热点~~hot-spots"，然后BLAST~~然后~~可以使用~~它~~该"hot-spots"来启动扩展，最终~~可能导致~~达到完全对齐的匹配。 对于核苷酸 - 核苷酸搜索（即，"blastn"），在启动延伸之前需要完整单词的精确匹配，~~使得通常~~从而可以通过增加或减小单词的大小从而来调节搜索的灵敏度和速度。 对于其他 BLAST 搜索，~~基于词之间的相似性考虑非精确词匹配~~则基于词之间的相似性对非精确词匹配进行了考虑。 相似性的量可以变化。 ~~网页允许字大小2,3和6。~~本网页版 Blast 允许字大小为2-15。

**Matrix:**

**打分矩阵：**

A key element in evaluating the quality of a pairwise sequence alignment is the "substitution matrix", which assigns a score for aligning any possible pair of residues. The theory of amino acid substitution matrices is described in [1], and applied to DNA sequence comparison in [2]. In general, different substitution matrices are tailored to detecting similarities among sequences that are diverged by differing degrees [1-3]. A single matrix may nevertheless be reasonably efficient over a relatively broad range of evolutionary change [1-3]. Experimentation has shown that the BLOSUM-62 matrix [4] is among the best for detecting most weak protein similarities. For particularly long and weak alignments, the BLOSUM-45 matrix may prove superior. A detailed statistical theory for gapped alignments has not been developed, and the best gap costs to use with a given substitution matrix are determined empirically. Short alignments need to be relatively strong (i.e. have a higher percentage of matching residues) to rise above background noise. Such short but strong alignments are more easily detected using a matrix with a higher "relative entropy" [1] than that of BLOSUM-62\. In particular, short query sequences can only produce short alignments, and therefore database searches with short queries should use an appropriately tailored matrix. The BLOSUM series does not include any matrices with relative entropies suitable for the shortest queries, so the older PAM matrices [5,6] may be used instead. For proteins, a provisional table of recommended substitution matrices and gap costs for various query lengths is:

评估成对序列比对的质量的关键因素是"置换矩阵"，其指定用于比对任何可能的残基对的分数。氨基酸取代矩阵的理论在[1]中描述，并应用于[2]中的DNA序列比较。一般来说，~~不同的替代矩阵被定制以检测以不同程度发散的序列之间的相似性~~定制不同的替换矩阵可以用来检测不同发散程度序列之间的相似性[1-3]。然而，~~单个矩阵在相对宽范围的进化变化上可能是相当有效的~~在相对较宽范围的进化改变上，单个矩阵可能是相当有效的[1-3]。实验表明，BLOSUM-62矩阵[4]是检测最弱蛋白相似性的最佳选择之一。对于特别长和弱的比对，BLOSUM-45矩阵~~可以证明是优越的~~被证明更具优越性。~~尚未开发带间隙比对的详细统计学理论~~用于空位比对更具体的统计学理论尚未被开发出来，且用于给定~~替代~~替换矩阵的最佳空位罚分是凭经验确定的。短比对需要相对强（即具有更高百分比的匹配残基）以升高到高于背景噪声。使用具有比BLOSUM-62高的"相对熵"[1]的矩阵更容易检测这种短而强的比对。特别地，短查询序列只能产生短~~对准~~比对，因此具有短查询的数据库搜索应当使用适当定制的矩阵。 BLOSUM系列不包括具有适于最短查询的相对熵的任何矩阵，因此可以使用较旧的PAM矩阵[5,6]。对于蛋白质，各种查询长度的推荐置换矩阵和空位罚分的临时表是：

|  **Query Length** | **Substitution Matrix**   | **Gap Costs**  |
| :------------: | :------------: | :------------: |
| **查询长度**  | **替换矩阵**  | **空位罚分**  |


##### **Gap Costs:**

The raw score of an alignment is the sum of the scores for aligning pairs of residues and the scores for gaps. Gapped BLAST and PSI-BLAST use "affine gap costs" which charge the score -a for the existence of a gap, and the score -b for each residue in the gap. Thus a gap of k residues receives a total score of -(a+bk); specifically, a gap of length 1 receives the score -(a+b).

比对的原始得分是用于~~比对残基~~残基对比对对和用于空位罚分得分的总和。 Gapped BLAST 和 PSI-BLAST 使用的"空位延伸罚分"，~~其对于缺口的存在对记分 -a进行计数~~对已经存在的一个空位记分为 -a ，~~并且对于缺口中的每个残基对记分-b进行计数~~对一个空位中的每一个残基则计分为 -b。  因此，k个残基的~~缺口~~空位~~接受~~获得的总分数为 - （a + bk）; 具体地，长度1的间隙~~接收~~获取的得分为 - （a + b）。

## 6.  BLAST Program Advanced Options

## BLAST 程序高级选项



