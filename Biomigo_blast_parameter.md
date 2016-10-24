## v-1.0 基本参数：
*   Expect threshold (E threshold)（evalue &lt;Real&gt;）：Expectation value (E) threshold for saving hits Default = `**<font color="#ff0000">10</font>**'.
*   Alignment view（outfmt &lt;String&gt;）：

   *   0 = Pairwise,
   *   5 = BLAST XML,
   *   6 = Tabular,

*   Alignments（num_alignments &lt;Integer &gt;）：Number of database sequences to show alignments for Default = `250'，* Incompatible with:  max_target_seqs

   *   250
   *   500
   *   1000
   *   2000
   *   20000

## v-1.1 新增参数 (参考)：
*   （**-word_size**）word_size <Integer>：Word size for wordfinder algorithm (length of best perfect match)

   设定字长大小，blastn为11， megablast 为28，其他为3。

   **ncbi blast：**
   *   blastn：7、11、15
   *   blastp：2、3、<font color="#ff0000">**6**</font>
   *   blastx：2、3、<font color="#ff0000" style="font-size: 18.4615px; line-height: 32.0123px;">**6**</font>
   *   tblastn：2、3、<font color="#ff0000" style="font-size: 18.4615px; line-height: 32.0123px;">**6**</font>
   *   tblastx：2、<font color="#ff0000">**3**</font></font>

   <font color="#ff0000">**biomingo_blast values：**</font>
   *   **2、3、6、7、11、15（或者：2 - 15）。**

*   （**-gapopen**）Cost to open a gap (zero invokes default behavior) \[Integer\] 

   起始空位罚分。

   **ebi blast：**(blastall 默认default=-1)

   *   blastn：default、0、1、2、5
   *   blastp：default、6-13、15、16、19
   *   blastx：default、6-13、15、16、19
   *   tblastn：default、6-13、15-16、19
   *   tblastx：default、6-13、15-16、19

   **<font color="#ff0000">biomingo_blast values：</font>**（以 NBK279675 为标准）

   *   **blastn：**(gapopen, gapextend) = (**5,2**(default) or **4,4** or **2,4** or **0,4** or **3,3** or **6,2** or **5,2** or **4,2** or **2,2**)
   *   **blastp：**(gapopen, gapextend) = (**11,2** or **10,2** or **9,2** or **8,2** or **7,2** or **6,2** or **13,1** or **12,1** or **11,1**(default) or **10,1** or **9,1**) 
   *   **blastx：**(gapopen, gapextend) = (**11,2** or **10,2** or **9,2** or **8,2** or **7,2** or **6,2** or **13,1** or **12,1** or **11,1**(default) or **10,1** or **9,1**)
   *   **tblastn：**(gapopen, gapextend) = (**11,2** or **10,2** or **9,2** or **8,2** or **7,2** or **6,2** or **13,1** or **12,1** or **11,1**(default) or **10,1** or **9,1**)
   *   **tblastx：**无
   

*   （**-E**, **-gapextend**）Cost to extend a gap \[Integer\] (default [-1] )

   空位延伸罚分。

   **ebi blast：**(blastall 默认default=-1)

   *   blastn：default、2
   *   blastp：1
   *   blastx：1
   *   tblastn：default、1
   *   tblastx：default、1

   **<font color="#ff0000">biomingo_blast values：</font>**（以 NBK279675 为标准）

   *   **blastn：**(gapopen, gapextend) = (**5,2**(default) or **4,4** or **2,4** or **0,4** or **3,3** or **6,2** or **5,2** or **4,2** or **2,2**)
   *   **blastp：**(gapopen, gapextend) = (**11,2** or **10,2** or **9,2** or **8,2** or **7,2** or **6,2** or **13,1** or **12,1** or **11,1**(default) or **10,1** or **9,1**) 
   *   **blastx：**(gapopen, gapextend) = (**11,2** or **10,2** or **9,2** or **8,2** or **7,2** or **6,2** or **13,1** or **12,1** or **11,1**(default) or **10,1** or **9,1**)
   *   **tblastn：**(gapopen, gapextend) = (**11,2** or **10,2** or **9,2** or **8,2** or **7,2** or **6,2** or **13,1** or **12,1** or **11,1**(default) or **10,1** or **9,1**)
   *   **tblastx：**无

*   （**-matrix**,**-M**）Comparison Matrix：default = BLOSUM62

   **ebi blast：**

   *   blastn：无
   *   blastp、blastx、tblastn、tblastx：BLOSUM45、BLOSUM50、BLOSUM62（default）、BLOSUM80、BLOSUM90、PAM30、PAM70、PAM250

   **<font color="#ff0000">biomingo_blast values(except blastn )：</font>**

   *   <font color="#ff0000">**BLOSUM45、BLOSUM50、BLOSUM62（default）、BLOSUM80、BLOSUM90、PAM30、PAM70、PAM250**</font>

